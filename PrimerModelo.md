# 📘 Primer Modelo de Semáforos Inteligentes en Cruce con Metro – UPPAAL

## 📜 Descripción del modelo

En este primer modelo se simula un cruce entre coches y metro. El objetivo es gestionar el flujo de tráfico y garantizar la seguridad en el cruce. Se implementan semáforos que regulan el paso de los coches y permiten a estos y al metro cruzar sin problemas. La principal funcionalidad de este modelo es la sincronización entre el semáforo de coches y el metro, asegurando que el metro solo cruce cuando no hay coches en el cruce, y que los coches solo crucen cuando el semáforo se lo permite (es decir, cuando el metro no está cruzando ni está cerca del cruce).

## 🧩 Descripción de autómatas

### 🚆 Metro

Simula el comportamiento de un metro en superficie que cruza la intersección.

**Estados:**
- `enLaParada`: El metro está detenido antes del cruce, en la parada. Es el estado inicial.
- `DentroCruce`: El metro ha entrado en el cruce.
- `Alarma`: Estado al que pasa el metro si intenta cruzar cuando hay coches en el cruce. En este estado, el metro no puede cruzar y debe esperar a que los coches se vayan.
- `FueraCruce`: El metro ha salido y permite restablecer el tráfico.

---

### 🚗 Coche

Representa un coche que se aproxima al cruce y obedece las señales del semáforo.

**Estados:**
- `Llegando`: El coche se aproxima al semáforo. Es el estado inicial.
- `EsperaPorRojo`: Si cuando intenta cruzar el semáforo está en rojo, espera en este estado.
- `Cruzando`: Cruza si el semáforo está en verde, es decir, si no hay metros en el cruce.
- `HaCruzado`: Ha completado el cruce y se reinicia el ciclo.

---

### 🚦 SemaforoCoche

Controla el estado del semáforo que regula el paso de los coches.

**Estados:**
- `Rojo`: No permite el paso de coches. Es el estado inicial, ya que los metros inician en la parada y tienen prioridad.
- `RojoCommit`: Estado commit intermedio entre rojo y verde.
- `Verde`: Permite el paso de coches si no hay metros en el cruce.
- `VerdeCommit`: Estado commit intermedio entre verde y rojo.

## ✅ Requisitos implementados

- [X] Los coches se detienen cuando el semáforo está en rojo.
- [X] El metro solo cruza cuando no hay coches en el cruce.
- [X] Sincronización del semáforo con la llegada del metro.
- [X] Gestión de múltiples coches en el cruce mediante contador.
- [X] Control del flujo de coches en ambos sentidos.
- [X] Semáforos también en ambos sentidos.

## ☑️ Propiedades verificadas

- [X] El sistema no se bloquea.
- [X] El Coche1 y el Coche2 cruzan eventualmente en todas las trazas.
- [X] El Metro1 y el Metro2 cruzan eventualmente en todas las trazas.
- [X] Ambos semáforos pueden estar en verde al mismo tiempo.
- [X] Ambos semáforos están sincronizados. Es decir, no puede estar uno en verde y otro en rojo. A veces no están ambos verdes o rojos a la vez debido a que hay estados "commited" entre Verde y Rojo.
- [X] Cuando el Coche1 está llegando, después en algún momento, cruza.
- [X] Cuando el Coche1 está en espera por rojo, después en algún momento, cruza.
- [X] Cuando el Metro1 está en la parada, después en algún momento, cruza.
- [X] Cuando el Metro1 está en alarma, después en algún momento, cruza.
- [X] Si el SemaforoCoche1 está en rojo, entonces estará en verde eventualmente.
- [X] En ningún momento hay algún coche y metro en el cruce al mismo tiempo.
- [X] Existe alguna combinacion con dos coches en el cruce a la vez, es posible.
- [X] Existe alguna combinacion con dos metros en el cruce a la vez, es posible.
- [X] La probabilidad de que el Coche1 llegue al estado cruzando en menos de 30 unidades de tiempo es alta (>95%).
- [X] La probabilidad de que el Coche2 llegue al estado cruzando en menos de 30 unidades de tiempo es alta (>95%).
- [X] La probabilidad de que el Metro1 llegue al estado de alarma en 1000 unidades de tiempo es baja (<5%). (Nos interesa que sea baja, ya que el metro no debería estar en alarma continuamente).
- [X] La probabilidad de que el Metro2 llegue al estado de alarma en 1000 unidades de tiempo es baja (<5%). (Nos interesa que sea baja, ya que el metro no debería estar en alarma continuamente).

## 💡 Consideraciones del diseño

- El metro tiene prioridad, no necesita semáforo para esta iteración, pero verifica condiciones de seguridad antes de cruzar.
- Se modelan múltiples instancias de coches y semáforos para simular concurrencia.
- Se usan canales para sincronización entre procesos (metroAproximando, metroSaliendo...).
- En semáforoCoche, al pasar de rojo a verde, se comprueba que no haya ningún metro en el cruce. Pero se hace con: Guard: _metrosEnCruce == 1_ Sync: _metroSaliendo?_. El 1 se debe a que este último metro es el que está saliendo del cruce y manda el _metroSaliendo!_.
- El flujo es "infinito". Los mismos coches y metros circulan ciclicamente por el cruce. Al llegar al final, suponemos que "reaparecen" al principio.
- Al ser "infinito" el flujo, las propiedades de verificación tardan un tiempo considerablemente grande en comprobarse. 
- En coche, de llegando a espera por rojo, en vez de _t >= tCocheLLegaSemaforoMin_, ponemos _t >= tCocheLLegaSemaforoMax_ para simular "ir despacito" porque ves el semáforo rojo de lejos.
- En cuanto a la dirección, 0 es ascendente y 1 es descendente.
- En cuanto a los semáforos, 0 es rojo y 1 es verde.
- Si nos interesara cambiar el estado inicial del metro a `dentroCruce`, deberíamos cambiar la variable _metrosEnCruce_ a 2 y _metrosEnParada_ a 0. Si no, el modelo no se comportará como esperamos.
- El tiempo que ha tardado mi ordenador en verificar las 19 propiedades ha sido de 7 minutos y 30 segundos.



## 🐞 Problemas encontrados y soluciones

| Problema                                      | Solución aplicada                                |
|----------------------------------------------|--------------------------------------------------|
| El uso de un boolean `enCruce` compartido, da problemas de sincronización ya que tenemos varios coches  | Reemplazado por __contador__ `cochesEnCruce`         |
| El canal `metroSaliendo` da problemas de sincronización similares al problema anterior, ya que tenemos 2 metros | Añadir un __contador__ `metrosEnCruce`   |
| Las propiedades de verificación no se cumplen si cambiamos el estado incial de metro a `EnParada` | El problema estaba en las propiedades globales, ya que iniciaba la variable __metrosEnCruce__ a 2 y __metrosEnParada__ a 0, y he tenido que intercambiarlas. |

## 📈 Posibles mejoras futuras

- Añadir nuevo semáforo, que se comunique con el semáforo superior, para gestionar el flujo de coches en el cruce inferior (Segunda iteración).
- Implementar un sistema de prioridad para ambulancias, que puedan cruzar sin esperar al semáforo, siempre comprobando las condiciones de seguridad (Tercera iteración).



## 🗃️ Archivos del repositorio

- `PrimerModelo.xml`: Primera iteración con metro, coches y semáforo básico. Archivo generado por y visible en UPPAAL.
- `PrimerModelo.md`: Documento de descripción del modelo.
