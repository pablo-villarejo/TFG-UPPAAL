# 📘 Primer Modelo de Semáforos Inteligentes en Cruce con Metro – UPPAAL

## 📜 Descripción del modelo

En este primer modelo se simula un cruce entre coches y un metro. El objetivo es gestionar el flujo de tráfico y garantizar la seguridad en el cruce. Se implementan semáforos que regulan el paso de los coches y permiten a estos y al metro cruzar sin problemas. La principal funcionalidad de este modelo es la sincronización entre el semáforo de coches y el metro, asegurando que el metro solo cruce cuando no hay coches en el cruce, y que los coches solo crucen cuando el semáforo se lo permite (es decir, cuando el metro no está cruzando ni está cerca del cruce).

## ✅ Requisitos implementados

- [X] Los coches se detienen cuando el semáforo está en rojo.
- [X] El metro solo cruza cuando no hay coches en el cruce.
- [X] Sincronización del semáforo con la llegada del metro.
- [X] Gestión de múltiples coches en el cruce mediante contador.
- [X] Control del flujo de coches en ambos sentidos.
- [X] Semáforos también en ambos sentidos.

## ☑️ Propiedades verificadas

- [X] El sistema no se bloquea.
- [X] El Coche1 y el Coche2 cruzan eventualmente.
- [X] El Metro1 y el Metro2 cruzan eventualmente.
- [X] Ambos semáforos pueden estar en verde al mismo tiempo.
- [X] Ambos semáforos están sincronizados. Es decir, no puede estar uno en verde y otro en rojo. A veces no están ambos verdes o rojos a la vez debido a que hay estados "commited" entre Verde y Rojo.
- [X] Si el Coche1 está llegando, entonces cruzará eventualmente.
- [X] Si el Metro1 está en la parada, entonces cruzará eventualmente.
- [X] Si el Metro1 está en Alarma, entonces cruzará eventualmente.
- [X] Si el SemaforoCoche1 está en rojo, entonces estará en verde eventualmente.
- [X] En ningún momento hay algún coche y metro en el cruce al mismo tiempo.
- [X] Existe alguna combinacion con dos coches en el cruce a la vez, es posible.
- [X] Existe alguna combinacion con dos metros en el cruce a la vez, es posible.

## 💡 Consideraciones del diseño

- El metro tiene prioridad, no necesita semáforo para esta iteración, pero verifica condiciones de seguridad antes de cruzar.
- Se modelan múltiples instancias de coches y semáforos para simular concurrencia.
- Se usan canales para sincronización entre procesos (metroAproximando, metroSaliendo...).
- En semáforoCoche, al pasar de rojo a verde, se comprueba que no haya ningún metro en el cruce. Pero se hace con: Guard: _metrosEnCruce == 1_ Sync: _metroSaliendo?_. El 1 se debe a que este último metro es el que está saliendo del cruce y manda el _metroSaliendo!_.
- El flujo es "infinito". Los mismos coches y metros circulan ciclicamente por el cruce. Al llegar al final, suponemos que "reaparecen" al principio.
- Al ser "infinito" el flujo, las propiedades de verificación tardan un tiempo considerablemente grande en comprobarse. 
- En coche, de llegando a espera por rojo, en vez de _t >= tCocheLLegaSemaforoMin_, ponemos _t >= tCocheLLegaSemaforoMax_ para simular "ir despacito" porque ves el semáforo rojo de lejos.
- En cuanto a la dirección, 0 es ascendente y 1 es descendente. En cuanto a los semáforos, 0 es rojo y 1 es verde. 



## 🐞 Problemas encontrados y soluciones

| Problema                                      | Solución aplicada                                |
|----------------------------------------------|--------------------------------------------------|
| El uso de un boolean `enCruce` compartido, da problemas de sincronización ya que tenemos varios coches  | Reemplazado por __contador__ `cochesEnCruce`         |
| El canal `metroSaliendo` da problemas de sincronización similares al problema anterior, ya que tenemos 2 metros | Añadir un contador `metrosEnCruce`   |

## 📈 Posibles mejoras futuras

- Añadir nuevo semáforo inferior para gestionar el flujo de coches en el otro cruce.

## 🗃️ Archivos del repositorio

- `PrimerModelo.xml`: Primera iteración con metro, coches y semáforo básico.
- `PrimerModelo.md`: Documento de descripción del modelo.
