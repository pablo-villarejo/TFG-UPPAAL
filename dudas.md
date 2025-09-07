
- ¿Es necesario explicar en la memoria las propiedades verificadas una por una?
    - Mi enfoque: No es necesario una por una, he explicado los conceptos generales que se han verificado y he añadido una imagen en la que se ven todas. Si se desea analizar o ver detalladamente alguna, para ello se entregan también los archivos del modelo. En la memoria priorizo información más relevante y mejor resumida.

- Ni CocheInferior.EsperaPorRojo --> CocheInferior.Cruzando ni CocheInferior.Llegando --> CocheInferior.Cruzando se cumplen en el segundo modelo.
    - No he conseguido encontrar el contraejemplo, en el simulador funciona correctamente y nunca se quedan sin cruzar. Antes podría resultar problemático la idea de añadir vueltas limitadas al metro, pero finalmente, sólo he añadido esa limitación a los coches. Seguiré investigando.
    - Solucionado.

- ¿Quizás realizar el tercer modelo usando de base el primero en vez de la versión reducida del segundo? O al menos, a la hora de comprobar las propiedades.
    - Ahorraría muchísimo tiempo en las verificaciones. Además, al circular las ambulancias por la calle principal, prácticamente no tendría interacción con los semáforos inferiores. Se podría hacer una versión tercera completa usando el segundo modelo completo como base, pero para las verificaciones, usar el primero.

- ¿Usar el metroSaliendo? de alguna forma en ambulancia?

- Modificar el semaforo de coches en el modelo 1 para hacerlo como en el 2, ahorrandome una transición commit.

- Añado o no esto: "En la Figura \ref{fig:mod1-metro} podemos ver los estados y transiciones de este autómata." al final de cada descripción de autómata?