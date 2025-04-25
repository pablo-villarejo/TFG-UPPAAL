# Semáforos Inteligentes con Verificación Formal (TFG - UMA)

Este repositorio contiene el Trabajo Fin de Grado (TFG) del grado en Ingeniería del Software de la Universidad de Málaga.

## Descripción del Proyecto

El proyecto tiene como objetivo el modelado, simulación y verificación formal de un sistema de **semáforos inteligentes** en un cruce real del **Campus de Teatinos (Universidad de Málaga)**: el cruce del **Boulevard Louis Pasteur con Jiménez Fraud**. Este cruce es especialmente crítico debido a su alto tráfico de **coches**, **peatones**, **metro en superficie** e incluso **ambulancias**, por su proximidad al Hospital Universitario Virgen de la Victoria.

Dada la complejidad y criticidad del cruce, se propone el uso de **métodos formales** para garantizar tanto la seguridad como la fluidez del tráfico. En concreto, se utilizará la herramienta **[UPPAAL](https://uppaal.org/)**, que permite modelar, simular y verificar sistemas de tiempo real mediante **Autómatas Temporizados (TA)** y **Autómatas Temporizados Estocásticos (STA)**.

## Metodología

El desarrollo del modelo se realizará por **iteraciones**: comenzando con un sistema básico, que se irá ampliando progresivamente. Cada iteración se almacenará como un archivo independiente dentro del repositorio, permitiendo consultar la evolución del sistema.

## Primera Iteración

En la primera versión del modelo se incluyen los siguientes elementos:

- Un **automata del metro** con 2 instancias.
- Un **semáforo de coches** con 2 instancias.
- Un **automata de coche**, con múltiples instancias.

Este modelo básico reproduce el comportamiento del cruce metro-carretera:

- Los coches circulan en ambos sentidos.
- Cuando el metro se aproxima, el semáforo para coches se pone en rojo.
- Los coches se detienen.
- El metro verifica que el cruce está libre antes de avanzar.

## Herramienta utilizada

- [UPPAAL](https://uppaal.org/)

---

> Cada iteración del modelo servirá para analizar propiedades del sistema usando model checking: seguridad (evitar colisiones), eficiencia (minimizar tiempos de espera), y robustez (resistencia ante variaciones en tiempos y eventos).