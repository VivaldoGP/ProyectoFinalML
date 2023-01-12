## Proyecto Final de la Asignatura Programación aplicada a la Geomática

El objetivo de este proyecto es la inclusión de la Inteligencia Artificial en el
campo de estudio de la Ingeniería Geomática, por lo que para este proyecto se realizará una **segmentación semántica** de imagenes satelitales de Dubai, en los Emiratos Arabes Unidos, buscando entrenar un modelo capaz de segmentar una imágen dada como input acorde a los datos de entrenamiento.

Como se mencionó a lo largo del curso, es necesario contar con datos de entrenamiento y prueba, en este caso los datos de prueba se toman de un dataset proporcionado por Kaggle.

Se puede acceder desde el siguiente enlace:

<https://www.kaggle.com/datasets/humansintheloop/semantic-segmentation-of-aerial-imagery?resource=download>

El dataset consiste en 72 imágenes areas de Dubai, las cuales están clasificadas en 6 clases principales, las cuales son:

1. Building: #3C1098
2. Land: #8429F6
3. Road: #6EC1E4
4. Vegetation: #FEDD3A
5. Water: #E2A929
6. Unlabeled: #9B9B9B

El dataset contiene las 72 imagenes y cada imagen tiene una máscara, la cual es la imagen clasificada con las 6 etiquetas.

Estas imágenes tienen dimensiones distintas, por lo que uno de los primeros pasos es normalizar estas dimensiones ya que el modelo admitirá unas dimensiones constantes, por lo que es necesario establecer el tamaño del patch, que será de *256*, teniendo así imágenes con dimensiones de *256x256*.El objetivo de esta reducción es dividir las imágenes en otras con las dimensiones deseadas y así amplicar el tamaño del dataset, logrando tener en total aproximadamente 1305 datos de entrenamiento. Se da por entendido que al mencionar datos se hace referencia a un *array*.

## Procedimiento

1. Se recorre el directorio en el que se encuentran las imagenes, estas se encuentran divididas en subcarpetas que a su vez se dividen en la imagen RGB y su respectiva máscara, por lo que se recorrde el directorio y se agrupan en listas que contengan los arrays para cada imagen.

2. Teniendo estas listas podemos realizar una visualización de las imagenes y sus mácaras para comprobar que los índices coinciden su representación es correcta.

3. Un paso importante es pasar las etiquetas de RGB a HEX, el sistema y su especificación se pueden encontrar [aquí](https://es.wikipedia.org/wiki/Sistema_hexadecimal).

4. Después se procede a obtener la cantidad de etiquetas con las que cuenta el dataset, buscando etiquetas únicas claramente, por lo que al final nos encontramos con una lista que va desde el 0 hasta el 5, por lo que estamos por buen camino, recordando que en un inicio se mencionó que el dataset contaba con 6 clases.

5. Se procede a pasar de las etiquetas a variables categoricas, teniendo como resultado un array con dimensiones explicadas a continuación:
*(1305, 256, 256, 6)*, donde 1305 es el número de datos(arrays), 256 son las dimensiones en alto y ancho, y 6 son las etiquetas.

6. Se selecciona un modelo para el entrenamiento, en este caso se opta por el *train_test_split*, el cual divide un array en subsets aleatorios de entrenamiento y prueba.

7. Definición de los pesos para cada una de las variables(etiquetas), en este caso al ser 6 variables se opta por dividir 1 entre en el número de variables, dando un peso para cada una de las variables de *0.1666*.

8. Para la elección de las funciones de perdida se eligieron ***Dice Loss*** y ***CategoricalFocalLoss***, esta última en especial para crear un criterio que mida la diferencia entre el valor de entrada y la predicción. Ambas funciones pueden ser consultadas en el siguiente [enlace](https://segmentation-models.readthedocs.io/en/latest/api.html?highlight=dice#segmentation_models.losses.CategoricalCELoss).

9. Para medir el grado de similitud entre los conjuntos verdaderos y las predicciones, se calcula el *coeficiente de Jaccard*, se puede consultar su teoría en el siguiente [enlace](https://es.wikipedia.org/wiki/%C3%8Dndice_de_Jaccard).

10. Un paso crucial es definir el modelo que será el encargado de procesar las entradas y pasarlas a través de las capas que se establezcan, la elección fue *UNET*, desarrollado por Olaf Ronneberger para la segmentación de imagenes Bio Médicas, la arquitectura contiene dos caminos, el primero de ellos(*encoder*) se encarga de capturar el contexto de la imagen, este encoder es un conjunto de capas convolucionales y max pooling. El segundo camino es la expansión simétrica que también es llamado decoder, que es usado para habilitar la localización precisa usando capas convolucionales transpuestas.

En el artículo original del autor el modelo se define de la siguiente manera:

![UNET_original](https://miro.medium.com/max/1400/1*OkUrpDD6I0FpugA_bbYBJQ.webp)
La arquitectura UNET descrita detalladamente se define a continuación:
![UNET_ARCH](https://miro.medium.com/max/1400/1*yzbjioOqZDYbO6yHMVpXVQ.webp)

11. Las capas del modelo se activan mediante la función de activación *relu*, exceptuando la salida que se emplea *softmax*, ambas se detallan muy bien en los siguientes enlances:
[relu](https://machinelearningmastery.com/rectified-linear-activation-function-for-deep-learning-neural-networks/) y [softmax](https://deepai.org/machine-learning-glossary-and-terms/softmax-layer)

12. Teniendo el modelo definido se procede a entrenar el modelo y calcular las metricas de precisión mediante el coeficiente de jaccard, empleando un optimizador que en este caso será *adam*.

## Conclusión

El modelo por si mismo y en su definición es adecuado y correcto acorde a sus objetivos, como cualquier modelo requiere entrenamiento y esta es la parte negativa del proyecto, debido a la cantidad de datos y las capas involucradas, como prueba se hicieron 2 iteraciones, ya que cada una tarda un aproximado de 8 minutos, no fui capaz de habilitar el procesamiento por gpu para que el proceso fuera más rápido y ampliar el número de epochs, aún así el resultado no es tan malo, teniendo una validación de poco mas 0.45 con solo dos iteraciones, entrando al notebook y dirigiendose al final se puede encontrar una comparación entre la imagen de prueba, la prueba etiquetada y su respectiva predicción.

Se deja de prueba la siguiente imagen para una vista previa del resultado.

![prediccion](https://github.com/VivaldoGP/ProyectoFinalML/blob/main/comparacion.png)

## Discusión

La Inteligencia Artificial tiene un potencial enorme en todos los campos de estudio pero precisamente en la Geomática tiene una gran utilidad, para este caso lo recomendado es aumentar el número de epochs para que el entrenamiento sea el suficiente y las predicciones tengan un mayor porcentaje de aciertos.

## Extras

En el siguiente [enlace](https://towardsdatascience.com/understanding-semantic-segmentation-with-unet-6be4f42d4b47) se puede encontrar una excelente explicación del tema desarrollado en este proyecto y que sirvió de inspiración y guía.


[presentacion](https://drive.google.com/file/d/1DB7f19DSi1xQwW5Y4cIIx4lIeUO2mK7_/view?usp=drivesdk)

