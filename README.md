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







