# 📘 Tercer Modelo de Semáforos Inteligentes en Cruce con Metro – UPPAAL

## 📜 Descripción del modelo

Las ambulancias son similares a los coches, pero tienen prioridad absoluta en el cruce. Cuando una ambulancia se acerca al cruce, no comprueba si los semáforos están en rojo o verde, sólo comprueba si hay algún metro cruzando. Si no hay ningún metro cruzando, la ambulancia puede cruzar el cruce inmediatamente. Si hay un metro cruzando, la ambulancia debe esperar a que el metro termine de cruzar antes de poder cruzar ella misma.


## 💡 Consideraciones del diseño

- 

\subsection{Descripción del entorno}

El caso de estudio corresponde al cruce entre Boulevard Louis Pasteur y la calle Jiménez Fraud, situado en el campus de Teatinos de la Universidad de Málaga. Se trata de un emplazamiento con un elevado volumen de tráfico rodado y con la complejidad añadida de la existencia de vías de metro en superficie y de un flujo habitual de vehículos de emergencia que acceden al cercano Hospital Universitario Virgen de la Victoria. Esta confluencia de actores y usos hace que la coordinación de la señalización semafórica sea especialmente crítica para minimizar riesgos y mantener la fluidez del tráfico. \par

\begin{figure}[H]
    \centering
    \includegraphics[width=0.8\textwidth]{images/cruce-maps-satelite.png}
    \caption{Imagen satélite del cruce entre Boulevard Louis Pasteur y Jiménez Fraud.}
    \label{fig:cruce-maps-satelite}
\end{figure}
 \par


