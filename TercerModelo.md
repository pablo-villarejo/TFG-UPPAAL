# 📘 Tercer Modelo de Semáforos Inteligentes en Cruce con Metro – UPPAAL

## 📜 Descripción del modelo

Las ambulancias son similares a los coches, pero tienen prioridad absoluta en el cruce. Cuando una ambulancia se acerca al cruce, no comprueba si los semáforos están en rojo o verde, sólo comprueba si hay algún metro cruzando. Si no hay ningún metro cruzando, la ambulancia puede cruzar el cruce inmediatamente. Si hay un metro cruzando, la ambulancia debe esperar a que el metro termine de cruzar antes de poder cruzar ella misma.


## 💡 Consideraciones del diseño

- Una ambulancia sin luces de emergencia encendidas se comportaría como un coche normal. El autómata ambulancia sólo representa una ambulancia en emergencia, con las luces de emergencia encendidas.

- Las ambulancias circulan por la calle Jiménez Fraud, no por el Boulevard Louis Pasteur.

- Puede parecer que para salir de \textit{CruceOCupado} la ambulancia funciona igual que un semáforo, y es lo mismo que esperar a que se ponga en verde. Esto no es así, debido a que el semáforo, para cambiar a verde, también verifica que no haya ningún metro en la parada. En cambio, la ambulancia puede cruzar en cuanto el metro haya salido del cruce, aunque haya un metro en la parada.

- Los tiempos de la ambulancia para reiniciar el ciclo son mucho mayores que los de un coche normal, ya que se supone que las ambulancias son menos comunes. En cambio, los tiempos de esta cuando circula, son considerablemente menores que los de un coche normal, ya que se supone que las ambulancias circulan a mayor velocidad. Los podemos ver en la Figura \ref{fig:mod3-dec}. 

- Al entrar en el cruce y al salir, actualiza la variable global \textit{ambulanciaEnCruce} para que se puedan comprobar si hay alguna ambulancia en el cruce.

- También limitamos las vueltas de los coches en la versión reducida de este modelo.

- El hecho de que las ambulancias no sean muy frecuentes, hace que el modelo no sea tan reutilizable como el de coche en ambas direcciones.

## 🐞 Problemas encontrados y soluciones

| Problema                                      | Solución aplicada                                |
|----------------------------------------------|--------------------------------------------------|
| En primera instancia, se consideró que como la ambulancia tenía prioridad absoluta, no necesitaba comprobar si había algún metro en el cruce. Esto provocaba que la ambulancia cruzara el cruce aunque hubiera un metro cruzando, lo cual no es seguro. | Añadir un estado intermedio en el que la ambulancia espera a que el metro termine de cruzar, en el caso de que haya uno presente. |
| Al igual que en el modelo anterior, el modelo completo era demasiado complejo para que Uppaal pudiera verificar todas las propiedades. | Crear la versión reducida del modelo, ya explicada en secciones anteriores. |
| Al principio, no se tuvo en cuenta que el metro comprobara si había alguna ambulancia en el cruce, como se hace con los coches. | Añadir un contador de ambulancias en el cruce que el metro consulte. Contador en vez de booleano, para permitir futuras mejoras en las que pueda haber más de una ambulancia. |



## 🗃️ Archivos del repositorio

- `TercerModelo.xml`: Tercer iteración, añadiendo ambulancias.
- `TercerModelo.md`: Documento de descripción del modelo.
- `TercerModeloReducido.xml`: Versión reducida del tercer modelo, con menos semáforos y limitando las vueltas de los coches principales.