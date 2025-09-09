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

- Como solución de esta iteración, creo que lo más correcto es suponer que, mientras los semáforos superiores de la calle Jiménez Fraud estén en verde, los semáforos inferiores de esa misma calle también deberían estarlo. Además, el semáforo del Boulevard Louis Pasteur estará en rojo durante todo ese tiempo. Este último sólo se pondrá en verde cuando los semáforos de Jiménez Fraud estén en rojo (porque esté pasando el metro). Esto nos dará un sistema seguro, aunque no muy eficiente quizás, ya que el tiempo de espera para los coches del Boulevard Louis Pasteur será considerablemente mayor. Esto se solucionará más adelante en esta iteración, o dependiendo de su complejidad, en iteraciones futuras.

- En la versión reducida del modelo, se ha optado por eliminar uno de los semáforos, el inferior de Jiménez Fraud. Además, se limitan las vueltas de los coches "principales". Para realizar las verificaciones, las realizamos con los semáforos superiores de Jiménez Fraud, ya que los inferiores no dejan de ser una extensión de los primeros.

- Se han ajustado los parámetros de tiempo de todos los actores para que todo funcione de la manera lo más fiel posible a la realidad.

- En el modelo completo, al modificar coche y añadirle un cruce más, haciendo que tenga dos, se podría pensar que deja de funcionar el hecho de usar un mismo autómata para coches que ciruclan en direcciones opuestas. Sin embargo, esto no es así, ya que al reproducir un bucle de coches infinito, siempre se suceden los mismos estados, viajen en el sentido que viajen. 

- Al igual que ocurre en el primer modelo con los coches, suponemos que los coches del Boulevard Louis Pasteur realizan un bucle infinito. Al cruzar, podría incorporarse al tráfico de Jiménez Fraud o cruzar hacia delante, pero para simplificar el modelo, suponemos que, al pasar cierto tiempo, el coche vuelve al punto de inicio.

- Como se ha visto en la  explicación de su autómata, la circulación coche de Jiménez Fraud en el modelo reducido se limita a dos vueltas. Esto nos permite comprobar las verficaciones necesarias reduciéndo considerablemente el número de estados y transiciones del modelo, y por tanto, el tiempo que tardan las verificaciones en completarse, como veremos en el capítulo de verificación y resultados.

## 🐞 Problemas encontrados y soluciones

| Problema                                      | Solución aplicada                                |
|----------------------------------------------|--------------------------------------------------|
| Las verificaciones tardan demasiado en completarse  | Crear un modelo 2 reducido con menos estados y transiciones (ESTE HA SIDO EL MAYOR PROBLEMA ENCONTRADO, EL MÁS IMPORTANTE) |

| Al añadir el límite de vueltas a los coches principales, el sistema a veces se bloqueaba | Ajustar algunos parámetros de tiempo, como los del metro para que no ocurra |

| Algunas propiedades de vivacidad de los nuevos coches no se cumplían | Relacionado con el problema anterior, no se cumplían porque el sistema se bloqueaba. Solucionado al ajustar los parámetros de tiempo |

| En el modelo reducido, al añadir el límite de vueltas a los coches principales, en ocasiones no se cumplía dicho límite | No solo añadir la guarda de transición hacia el estado Fin, sino también una guarda por cada otra transición posbile, para evitar que realizara otras transiciones al llegar a la vuelta límite |


## 📈 Posibles mejoras futuras

- Añadir...

## 🗃️ Archivos del repositorio

- `SegundoModelo.xml`: Segunda iteración, añadiendo nuevos semáforos en la parte inferior de la calle.
- `SegundoModelo.md`: Documento de descripción del modelo.
- `SegundoModeloReducido.xml`: Versión reducida del segundo modelo, con menos semáforos y limitando las vueltas de los coches principales.