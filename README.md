# Clasificacion-y-deteccion-de-topicos-en-Twitter-empleando-BerTopic
Se lleva a cabo un análisis de las temáticas discutidas en Colombia en la red social Twitter durante el mes de junio de 2022. Para ello, se implementan dos estructuras concatenadas basadas en procesamiento de lenguaje natural específicamente Bertopic

Título del Proyecto: Análisis y Detección de Temas en Twitter durante las Elecciones Presidenciales de Colombia 2022: Un Estudio de Caso

Resumen: En esta investigación, se realiza un análisis de las temáticas discutidas en Colombia a través de la plataforma de redes sociales Twitter durante el mes de junio de 2022. Para lograr este objetivo, se implementan dos estructuras secuenciales basadas en el procesamiento de lenguaje natural. La primera estructura se fundamenta en técnicas de aprendizaje automático y el uso de modelos Transformers para identificar temas globales, como política, economía y cultura. Por otro lado, la segunda estructura emplea modelos no supervisados, específicamente la técnica Bertopic, ajustando los hiperparámetros mediante los algoritmos UMAP y HDBSCAN. Los resultados experimentales del modelo supervisado arrojan una precisión del 0,81%, un Recall del 0,851% y un F1 Score de 0,872. En contraste, el modelo no supervisado Bertopic obtiene un rendimiento de 0,445 en CV y -1,03 en UMASS. Este estudio demuestra la capacidad de analizar y comprender las opiniones expresadas en la sociedad digital, así como identificar y segmentar los temas más relevantes. Las aplicaciones potenciales de esta investigación abarcan desde modelos predictivos de noticias hasta estrategias de publicidad dirigida y análisis político.

# Autores
A continuación, se presenta el listado de autores y su respectivo correo electrónico.
| Organización         | Nombre del Miembro           | Correo Electrónico               |
|----------------------|-----------------------------|---------------------------------|
| PUJ-Bogotá           | Luis Gabriel Moreno Sandoval | morenoluis@javeriana.edu.co     |
| PUJ-Bogotá           | Cesar Ramirez Gomez          | ce-ramirez@javeriana.edu.co     |

# Análisis exploratorio base de datos modelo supervisado

Para entrenar el modelo supervisado de detección de temas, se utilizó como fuente de datos el sitio web de noticias "El Tiempo" [55], un periódico colombiano fundado el 30 de enero de 1911 por Alfonso Villegas Restrepo. En la actualidad, El Tiempo goza de una amplia circulación en Colombia, y su archivo de noticias resultó ser una fuente de datos invaluable para este estudio.
Las noticias recopiladas de El Tiempo se clasificaron en las siguientes categorías: deportes, política, cultura, vida, economía y tecnosfera. Se recopilaron datos de los años comprendidos entre 2020 y 2023, con los siguientes volúmenes de noticias por año:
- 2020: 3487 noticias
- 2021: 8010 noticias
- 2022: 8887 noticias
- 2023: 2116 noticias

A lo largo de estos años, se observó que la categoría de deportes presentó la mayor cantidad de noticias acumuladas, alcanzando un porcentaje del 47,99%. En segundo lugar, se encontró la categoría de cultura, que representó un 34,07% del total. La categoría de política ocupó el tercer lugar con un 7,70% de las noticias acumuladas. La economía ocupó el cuarto puesto, representando un 5,16% de las noticias. La categoría de vida se ubicó en el quinto lugar, con un porcentaje del 4,01%. Finalmente, la tecnosfera registró el porcentaje más bajo de noticias acumuladas, con un 1,07%. Estos datos demuestran la diversidad de temas cubiertos por El Tiempo y proporcionan una base sólida para entrenar el modelo supervisado en la detección de temas en Twitter durante las Elecciones Presidenciales de Colombia 2022.
![image](https://github.com/cesra147/Clasificacion-y-deteccion-de-topicos-en-Twitter-empleando-BerTopic/assets/99692504/e6911a7d-84cb-4bc6-89f3-427f4de14b57)

A continuación, se presenta el análisis de la frecuencia de palabras en todas las categorías, que se muestra en la Tabla 1:

![image](https://github.com/cesra147/Clasificacion-y-deteccion-de-topicos-en-Twitter-empleando-BerTopic/assets/99692504/7d2e7ed9-e347-4e49-9179-ec94871ca71f)

Se examina el comportamiento de las noticias a lo largo del año 2022, desglosado por meses, a través de una representación gráfica. Se puede observar que las categorías predominantes a lo largo de todo el período son "deportes" y "cultura", las cuales muestran una intersección que culmina en el mes de junio.

![image](https://github.com/cesra147/Clasificacion-y-deteccion-de-topicos-en-Twitter-empleando-BerTopic/assets/99692504/0fc7dc76-3a7c-4d62-bb6f-b71e65345e57)

# Experimentación de modelos para la detección de temas

En la experimentación de modelos para la detección de temas, se aplicaron los siguientes métodos de aprendizaje automático: Naive Bayes, regresión logística, XGBoost y el modelo basado en Transformers Bert, con el objetivo de comparar su rendimiento en las métricas de precisión, recall y F1 score. El conjunto de datos se dividió en un 80% para entrenamiento, lo que equivale a 4200 datos, y un 20% para pruebas, lo cual representa 1800 datos. Para crear la bolsa de palabras, se seleccionaron las 10,000 palabras más relevantes del texto, junto con monogramas, bigramas y trigramas.

a) Modelos de aprendizaje automático:

Naive Bayes: Se definió un algoritmo de clasificación con distribución multinomial.
Regresión logística: Se creó un objeto de regresión logística utilizando el solucionador 'liblinear'.
XGBoost: Se creó un objeto de clasificador utilizando la clase XGBRFClassifier de la biblioteca XGBoost. Se definieron los siguientes hiperparámetros: tasa de aprendizaje (0.1), número de iteraciones (1000), profundidad máxima de cada árbol (10) y proporción de características a considerar en cada árbol (0.7).
b) Modelo Transformers Bert:
Se utilizó un codificador universal de oraciones llamado "universal-sentence-encoder-cmlm/multilingual-basemodel", capaz de trabajar con más de 100 idiomas, y se seleccionó el idioma español. Para convertir el texto en vectores de alta dimensión, se cargaron las capas de preprocesamiento y codificación desde TensorFlow Hub, creando una función sencilla para obtener las representaciones del texto de entrada.

El modelo se estructuró con capas de preprocesamiento y codificación, seguidas de una capa de eliminación aleatoria "dropout" con un valor de 0.2 y una capa densa con una función de activación Softmax. El modelo se entrenó durante 20 épocas, y se utilizó la función "EarlyStopping" para monitorear la pérdida de validación durante el entrenamiento. Si la métrica no mejoraba durante al menos 3 épocas, el entrenamiento se detenía y se restauraban los pesos de la época en la que la pérdida de validación presentaba el mejor valor.

En la Tabla II se presentan los resultados de los modelos:

![image](https://github.com/cesra147/Clasificacion-y-deteccion-de-topicos-en-Twitter-empleando-BerTopic/assets/99692504/70ab8cbb-b416-49f4-a63d-6d3a7c82eae9)
Se seleccionó el modelo Bert debido a su equilibrio en las métricas de precisión, recall y puntuación F1, tanto globales como en cada clase individual. Este modelo muestra una falta de sesgo entre las diferentes clases, un aspecto que es importante para el análisis imparcial de los datos. Además, en trabajos futuros, se puede mejorar su rendimiento al cargar modelos preentrenados desde TensorFlow Hub y ajustarlos según las necesidades específicas, como la clasificación, incrustaciones, arquitectura, idioma, entre otros.

# Aplicación del Modelo Supervisado
Utilizando el modelo BERT, se clasificó una base de datos compuesta por 1,801,635 tuits de usuarios colombianos durante el mes de junio de 2022. Se generó la siguiente frecuencia por categorías: cultura (704,947 tuits), política (361,315 tuits), deportes (243,031 tuits), vida (222,994 tuits), tecnosfera (156,864 tuits) y economía (112,484 tuits).
![image](https://github.com/cesra147/Clasificacion-y-deteccion-de-topicos-en-Twitter-empleando-BerTopic/assets/99692504/c65697f9-40d7-46a3-bef0-484f64d9a384)

# Experimentación para la generación de típicos de temas
Durante el proceso de construcción del modelo, se evaluaron los siguientes hiperparámetros que componen la librería Bertopic, comparando las variables CV y UMASS. El conjunto de datos está constituido por tuits relacionados con el país Colombia en el mes de junio y la categoría política. A este conjunto de datos se le aplicó la técnica de preprocesamiento y lematización. A continuación, se describen los parámetros empleados durante la experimentación:
I. Agrupación de documentos: se emplea el modelo "paraphrase-multilingual-MiniLM-L12-v2" para el idioma español en todos los experimentos.
II. Reducción de dimensionalidad: se establecen los hiperparámetros del algoritmo UMAP en las siguientes combinaciones:
a. Número de vecinos: 10, 15 y 20.
b. Distancia mínima: 0.0 y 0.1.
c. Número de componentes: 2, 5 y 8.
d. Métrica: se utiliza la métrica Coseno.

III. Agrupación: se configura el algoritmo HDBSCAN modificando el tamaño mínimo del clúster en 10, 15 y 20.
IV. Bolsa de palabras: se emplea la librería Count Vectorizer [58] para el idioma español.
V. Número de temas: "Auto" determina independientemente el número máximo de temas basado en el algoritmo HDBSCAN para todos los experimentos.
VI. N-gramas: se define la detección de monogramas, bigramas y trigramas para todos los experimentos.

Se llevaron a cabo 54 experimentos con los siguientes resultados, agrupados por el tamaño mínimo del clúster

![image](https://github.com/cesra147/Clasificacion-y-deteccion-de-topicos-en-Twitter-empleando-BerTopic/assets/99692504/97ee6ad7-e136-44e0-8f7b-bf5cc940650b)
![image](https://github.com/cesra147/Clasificacion-y-deteccion-de-topicos-en-Twitter-empleando-BerTopic/assets/99692504/b6a8411c-998b-49fa-99db-821a6f015c47)
![image](https://github.com/cesra147/Clasificacion-y-deteccion-de-topicos-en-Twitter-empleando-BerTopic/assets/99692504/67308fbc-94f4-4e64-b2d9-8522995b80c5)

El experimento número 51 arrojó los valores más altos tanto en la métrica CV (0.393) como en la métrica UMASS (-0.579). Vale la pena destacar la combinación de un número de vecinos igual a 20, un número de componentes igual a 8 y un tamaño mínimo de clúster igual a 20, ya que esta configuración generó un buen desempeño en las métricas combinadas.
Sin embargo, el experimento que obtuvo el mejor resultado entre las 54 combinaciones fue el número 15. En este experimento, se utilizó un tamaño mínimo de clúster igual a 20, un número de vecinos igual a 20, un número de componentes igual a 5 y una distancia mínima igual a 0. Los valores obtenidos fueron un CV de 0.45 y un UMASS de -0.44. Estos parámetros aseguran que los temas generados automáticamente por el modelo sean coherentes y que las palabras clave extraídas sean altamente relevantes y representativas del contenido analizado.

# Aplicación del modelo Bertopic Sintonizado

Durante el mes de junio de 2022, se llevó a cabo el entrenamiento del modelo mediante la generación automática de temas relacionados con la clase política. Como resultado automático, se obtuvieron un total de 752 tópicos. Los temas más populares, según la cantidad de tuits generados, fueron los siguientes:

- Tópicos 1: 8000 tuits
- Tópicos 0: 7886 tuits
- Tópicos 2: 6663 tuits
- Tópicos 5: 6190 tuits
- Tópicos 4: 5968 tuits
Por otro lado, se identificaron los temas menos frecuentes, los cuales tuvieron una menor cantidad de tuits:
- Tópicos 699: 27 tuits
- Tópicos 717: 28 tuits
- Tópicos 611: 34 tuits
- Tópicos 593: 38 tuits
- Tópicos 513: 39 tuits

La distribución de los tópicos muestra una notable variación en la cantidad de tuits asociados, evidenciando la existencia de temas con mayor y menor participación. Algunos tópicos alcanzaron una alta frecuencia de tuits, con valores superiores a 5,000, mientras que otros presentaron una frecuencia mínima, con menos de 50 tuits asociados. En términos estadísticos, se observa una media de frecuencia de 480 tuits, una mediana de 224.0 tuits y una moda de 146 tuits. Estos valores indican una dispersión significativa en la frecuencia de los tuits, con una presencia destacada de temas con alta frecuencia y la existencia de temas menos comunes.

![image](https://github.com/cesra147/Clasificacion-y-deteccion-de-topicos-en-Twitter-empleando-BerTopic/assets/99692504/2390993d-a5b6-471b-9991-9f9dca347fbe)

Con el modelo de tópicos generales ya establecido, continuamos con la representación en cada paso en el tiempo. Para garantizar una visualización y rendimiento óptimos, configuramos el modelo únicamente con los 40 tópicos de mayor frecuencia. Esta elección se basa en la recomendación del autor de la biblioteca Bertopic [32]. Activamos los parámetros de "afinación global" y "afinación evolutiva". Los resultados se representan en la siguiente línea de tiempo, donde el número de tuits se encuentra en el eje y y las fechas en el eje x. Los nombres generados por Bertopic se determinan según la frecuencia de las palabras y se representan mediante colores distintivos en la siguiente gráfica.

![image](https://github.com/cesra147/Clasificacion-y-deteccion-de-topicos-en-Twitter-empleando-BerTopic/assets/99692504/1040ae74-c524-41e1-9173-b1c9e924f209)

La gráfica presenta un comportamiento dinámico en la frecuencia de tuits para los tópicos analizados. Destacan el tópico 0, denominado "fajardo bogota sergio claudia", y el tópico 1, denominado "votar votar petro voto petro", con una alta frecuencia a lo largo de toda la línea temporal. Se evidencia un cambio en la frecuencia en la mayoría de los tópicos a medida que se acerca la fecha del 19 de junio, especialmente pasando el mediodía. Esto resalta la naturaleza cambiante y dinámica de los tópicos analizados.

![image](https://github.com/cesra147/Clasificacion-y-deteccion-de-topicos-en-Twitter-empleando-BerTopic/assets/99692504/04464099-7fba-4f47-905f-a6ad03f2281c)

El 19 de junio se llevó a cabo la segunda vuelta de las elecciones presidenciales, en las cuales participaron los candidatos Gustavo Petro y su compañera de fórmula Francia Márquez, quienes obtuvieron el 50,44% de los votos, y Rodolfo Hernández y su fórmula vicepresidencial Marelen Castillo, quienes obtuvieron el 47,31% de los votos [59]. Este evento electoral tuvo un impacto significativo en la discusión en línea y se refleja en la variación de los tópicos analizados.

![image](https://github.com/cesra147/Clasificacion-y-deteccion-de-topicos-en-Twitter-empleando-BerTopic/assets/99692504/e3c597ae-73bb-4457-87ce-1008f60e5dad)

Para comprender su naturaleza evolutiva, se selecciona el tópico 1 como caso de estudio, examinando su trayectoria temporal. Inicialmente, se caracteriza por una variabilidad en la frecuencia de tuits, oscilando entre 100 y 230, lo que representa su etapa inicial. A partir del 17 de junio, experimenta un incremento constante y alcanza su punto máximo el día 19 de junio a las 18 horas. Posteriormente, se observa un declive abrupto llegando a aproximadamente 50 tuits con una tendencia descendente.  El día 29 de junio, la frecuencia disminuye aún más, aproximándose a cero, lo que podría indicar el final del tema.




