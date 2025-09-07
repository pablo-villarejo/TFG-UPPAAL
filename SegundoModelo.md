# 📘 Segundo Modelo de Semáforos Inteligentes en Cruce con Metro – UPPAAL

## 📜 Descripción del modelo

En este segundo modelo, se trabaja usando el modelo anterior como base. Se añade, principalmente, nuevos semáforos en la parte inferior de la calle Jiménez Fraud. Para ello, hay que añadir otro modelo, no instancia, de semáforo, ya que el semáforo del modelo anterior estaba pensado para comunicarse con el metro únicamente. Este nuevo semáforo, se comunica con otros semáforos de coche que gestionan el flujo de coches en el cruce.

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

- [X] Todos los requisitos mencionados en el modelo anterior.

## ☑️ Propiedades verificadas

- [X] Todas las propiedades del modelo anterior (ajustando los nombres a los de los nuevos estados). 
- [X] El coche inferior puede cruzar eventualmente.
- [X] La probabilidad 

## 💡 Consideraciones del diseño

- Como primera solución de esta iteración, creo que lo más correcto es suponer que, mientras los semáforos superiores de la calle Jiménez Fraud estén en verde, los semáforos inferiores de esa misma calle también deberían estarlo. Además, el semáforo del Boulevard Louis Pasteur estará en rojo durante todo ese tiempo. Este último sólo se pondrá en verde cuando los semáforos de Jiménez Fraud estén en rojo (porque esté pasando el metro). Esto nos dará un sistema seguro, aunque no muy eficiente, ya que el tiempo de espera para los coches del Boulevard Louis Pasteur será considerablemente mayor. Esto se solucionará más adelante en esta iteración, o dependiendo de su complejidad, en iteraciones futuras.

- Suponemos que los coches en el cruce inferior cruzarán relativamente rápido (máx = 7 y min = 2), lo que nos ayudará a minimizar el tiempo de espera en el semáforo.

- En la versión reducida del modelo, se ha optado por eliminar uno de los semáforos, el inferior de Jiménez Fraud. Además, se limitan las vueltas de los coches "principales". Para realizar las verificaciones, las realizamos con los semáforos superiores de Jiménez Fraud, ya que los inferiores no dejan de ser una extensión de los primeros.

## 🐞 Problemas encontrados y soluciones

| Problema                                      | Solución aplicada                                |
|----------------------------------------------|--------------------------------------------------|
| Las verificaciones tardan demasiado en completarse  | Crear un modelo 2 reducido con menos estados y transiciones |
| Al añadir el límite de vueltas a los coches principales, el sistema a veces se bloqueaba | Ajustar los tiempos del metro para que no ocurra |



## 📈 Posibles mejoras futuras

- Añadir...

## 🗃️ Archivos del repositorio

- `SegundoModelo.xml`: Segunda iteración, añadiendo nuevos semáforos en la parte inferior de la calle.
- `SegundoModelo.md`: Documento de descripción del modelo.


🔹 Básicas (comportamiento esperado)

 El CocheInferior1 y el CocheInferior2 cruzan eventualmente en todas las trazas.

 Cuando el CocheInferior1 está llegando, después en algún momento cruza.

 Cuando el CocheInferior1 está en espera por rojo, después en algún momento cruza.

 Si el SemaforoCocheInferior está en rojo, entonces estará en verde eventualmente.

🔹 Intermedias (propiedades de consistencia)

 En ningún momento hay un CocheInferior y un coche de la calle principal cruzando en el mismo cruce inferior al mismo tiempo.

 Los semáforos inferiores están sincronizados con los semáforos superiores (si los superiores están en verde, los inferiores también lo están).

 Existe alguna traza en la que dos coches inferiores están cruzando a la vez.

 Existe alguna traza en la que un coche inferior y un coche superior están cruzando a la vez, pero en cruces distintos (permitido).

🔹 Probabilísticas (rendimiento/eficiencia)

 La probabilidad de que el CocheInferior1 llegue al estado cruzando en menos de 50 unidades de tiempo es alta (>95%). (plazo más largo que los coches de la principal, para reflejar su peor acceso)

 La probabilidad de que el CocheInferior2 llegue al estado cruzando en menos de 50 unidades de tiempo es alta (>95%).

 La probabilidad de que un coche inferior tenga que esperar más de 200 unidades de tiempo en el semáforo es significativa (>30%). (espera larga, porque depende del metro que fuerza el rojo en la principal)

 La probabilidad de que un coche inferior cruce sin detenerse (pasa de "LlegandoInf" a "CruzandoInf" sin pasar por "EsperaInf") es baja (<20%). (ya que casi siempre le tocará esperar al depender del metro)