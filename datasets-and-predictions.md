---
description: Itera sobre los conjuntos de datos y entiende las predicciones del modelo.
---

# Datasets & Predictions \[Early Access\]

Actualmente, esta característica está en una fase de acceso anticipado. Puedes usarla en nuestro servicio de producción en wandb.ai, con [algunas limitaciones](https://docs.wandb.com/untitled#current-limitations). La API está sujeta a cambios. ¡Nos encantaría oír preguntas, comentarios, e ideas! Escríbenos a [feedback@wandb.com](mailto:feedback@wandb.com).

Los datos son la parte central de cada entono de trabajo de ML. Hemos agregado nuevas características poderosas a los Artefactos de W&B para permitirte visualizar y consultar conjuntos de datos, y modelar evaluaciones a nivel de ejemplo. Puedes usar esta nueva herramienta para analizar y entender tus conjuntos de datos, y para medir y depurar el desempeño del modelo.

Métete de lleno y prueba una demostración de principio a fin: [![](https://colab.research.google.com/assets/colab-badge.svg)](http://wandb.me/dsviz-demo-colab)

![Here&apos;s a preview of the Datasets &amp; Predictions dashboard](https://paper-attachments.dropbox.com/s_21D0DE4B22EAFE9CB1C9010CBEF8839898F3CCD92B5C6F38DBE168C2DB868730_1605673880422_image.png)

## Cómo funciona

 Nuestro objetivo es darte herramientas altamente escalables, flexibles y configurables, con visualizaciones enriquecidas, listas para ser usadas, disponibles para las tareas comunes. El sistema está construido a partir de:

* La capacidad de guardar objetos wandb.Table grandes, conteniendo opcionalmente medios enriquecidos \(como imágenes con cajas delimitadoras\), dentro de los Artefactos de W&B.
* El soporte para las referencias a los archivos de artefactos cruzados, y la capacidad de unir tablas en la interfaz de usuario. Esto es usado, por ejemplo, para registrar un conjunto de predicciones de cajas delimitadoras contra un artefacto de un conjunto de datos reales, sin duplicar las imágenes originales y las etiquetas.
* \[en el futuro\] El soporte de una API Backend para consultas a gran escala sobre las tablas almacenadas en los Artefactos de W&B.
* Una “arquitectura de paneles de la interfaz de usuario digitados e intercambiables en tiempo real” completamente nueva. Esto es lo que impulsa a las visualizaciones enriquecidas y a los gráficos que ves mientras comparas y agrupas tu tablas de datos. Eventualmente, vamos a abrirlo para que los usuarios puedan agregar visualizadores completamente personalizados, que funcionen en cualquier lugar de la interfaz de usuario de W&B.

## UI

_Prosigue abriendo este_ [_proyecto de ejemplo_](https://wandb.ai/shawn/dsviz_demo/artifacts/dataset/train_results/18bab424be78561de9cd/files)_, que fue generado desde nuestra_ [_colab de demostración_](http://wandb.me/dsviz-demo-colab)_._

Para visualizar las tablas registradas y los objetos de medios, abre un artefacto, ve a la pestaña Archivos, y haz click en la tabla o en el objeto. Intercambia a la **Vista del gráfico** para ver cómo son conectados los artefactos y las ejecuciones en el proyecto. Cambia Explotar para ver las ejecuciones individuales de cada paso en el pipeline.

###  ****Visualizando tablas

Abre la pestaña Archivos para ver la interfaz de usuario de las visualizaciones principales. En el [proyecto de ejemplo](https://wandb.ai/shawn/dsviz_demo/artifacts/dataset/train_results/18bab424be78561de9cd/files), haz click en “train\_iou\_score\_table.table.json” para visualizarlo. Para explorar datos, puedes filtrar, agrupar y ordenar la tabla.

![](.gitbook/assets/image%20%2899%29.png)

###  Filtrado

 Los filtros son especificados en el [lenguaje de agregación de mongodb](https://docs.mongodb.com/manual/meta/aggregation-quick-reference/), que tiene buen soporte para hacer consultas sobre objetos anidados \[además: ¡en realidad no usamos mongodb en nuestro backend!\]. Aquí hay dos ejemplos: 

**Encuentra los ejemplos que tengan &gt; 0.05 en la columna “road”**

`{$gt: ['$0.road', 0.05]}` 

**Encuentra los ejemplos que tengan una o más predicciones de cajas delimitadoras con “car”**

`{  
  $gte: [   
    {$size:   
      {$filter:   
        {input: '$0.pred_mask.boxes.ground_truth',  
          as: 'box',  
          cond: {$eq: ['$$box.class_id', 13]}  
        }  
      }  
    },  
  1]  
}`

### Agrupamiento

 Intenta agrupar por “dominant\_pred”. Vas a ver que las columnas numéricas se vuelven automáticamente histogramas cuando la tabla es agrupada.

![](https://paper-attachments.dropbox.com/s_21D0DE4B22EAFE9CB1C9010CBEF8839898F3CCD92B5C6F38DBE168C2DB868730_1605673736462_image.png)

\*\*\*\*

### Ordenamiento

Haz click en Ordenar y elige cualquier columna a partir de la cual quieras ordenar la tabla.

### Comparación

Compara dos versiones cualquiera de un artefacto en la tabla. Pasa el puntero del mouse sobre “v3” en la barra lateral, y haz click en el botón “Comparar”. Esto te va a mostrar las predicciones de ambas versiones en una tabla simple. Piensa como si ambas tablas estuviesen superpuestas entre sí. La tabla decide representar gráficos de barra para las columnas numéricas entrantes, con una barra por cada tabla que está siendo comparada.

Puedes utilizar los “Filtros rápidos” de arriba para limitar los resultados sólo a los ejemplos que estén presentes en ambas versiones.

![](https://paper-attachments.dropbox.com/s_21D0DE4B22EAFE9CB1C9010CBEF8839898F3CCD92B5C6F38DBE168C2DB868730_1605673764298_image.png)

  
Prueba comparar y agrupar a la vez. Vas a obtener un histograma múltiple, en donde usamos un color por cada tabla entrante.  


![](https://paper-attachments.dropbox.com/s_21D0DE4B22EAFE9CB1C9010CBEF8839898F3CCD92B5C6F38DBE168C2DB868730_1605673664913_image.png)

## API de Python

Prueba nuestra [colab de demostración](http://wandb.me/dsviz-demo-colab) para ver un ejemplo de principio a fin.

 Para visualizar los conjuntos de datos y las predicciones, registra medios enriquecidos en un artefacto. En adición a guardar los archivos sin procesamiento en los artefactos de W&B, ahora también puedes guardar, recuperar y visualizar otros tipos de medios enriquecidos provistos por la API de wandb.

 Actualmente, son soportados los siguientes tipos:

* wandb.Table\(\)
* wandb.Image\(\)

Pronto habrá soporte para tipos de medios adicionales.

###  ****Nuevos métodos de los artefactos

Hay dos nuevos métodos sobre los objetos Artifact:

`artifact.add(object, name)`

* Agrega un objeto de medios a un artefacto. Los tipos actualmente soportados son wandb.Table y wandb.Image, viniendo más próximamente.
* Esto agrega recursivamente cualquier objeto y activo de medios \(como archivos ‘.png’\) al artefacto.

`artifact.get(name)`

* Devuelve un objeto de medios reconstruido a partir de un artefacto almacenado.

Estos métodos son simétricos. Puedes almacenar un objeto en el artefacto usando .add\(\), y asegurarte de obtener exactamente el mismo objeto usando .get\(\), sobre cualquier máquina sobre la que necesites hacerlo.

### Objetos de medios wandb.\*

`wandb.Table`

Las tablas son una parte central de las visualizaciones de los conjuntos de datos y de las predicciones. Para visualizar un conjunto de datos, ponlo en una wandb.Table, agregando objetos wandb.Image, arreglos, diccionarios, strings y números donde sea necesario, y entonces agrega tu tabla a un artefacto. Actualmente, cada tabla está limitada a 50.000 filas. Puedes registrar en un artefacto tantas tablas como desees.

El siguiente código de ejemplo guarda como un wandb.Table a 1000 imágenes y etiquetas del conjunto de datos de test cifar10 de Keras, adento de un artefacto.

```python
import tensorflow as tf
import wandb

classes = ['airplane', 'automobile', 'bird', 'cat',
           'deer', 'dog', 'frog', 'horse', 'ship', 'truck']
_, (x_test, y_test) = tf.keras.datasets.cifar10.load_data()

wandb.init(job_type='create-dataset') # start tracking program execution

# construct a table containing our dataset
table = wandb.Table(('image', 'label'))
for x, y in zip(x_test[:1000], y_test[:1000]):
    table.add_data(wandb.Image(x), classes[y[0]])

# put the table in an artifact and save it
dataset_artifact = wandb.Artifact('my-dataset', type='dataset')
dataset_artifact.add(table, 'dataset')
wandb.log_artifact(dataset_artifact)
```

Después de correr este código, vas a ser capaz de visualizar la tabla en la interfaz de usuario de W&B. Haz click en “dataset.table.json”, en la pestaña Archivos del artefacto. Intenta agrupar por “label” para obtener los ejemplos de cada clase en la columna “image”.

  
`wandb.Image`

Puedes construir objetos wandb.Image, como se describe en nuestra [documentación de wandb.log](https://docs.wandb.com/library/log#images-and-overlays).

wandb.Image te permite adjuntar máscaras de segmentación y cajas delimitadoras a las imágenes, como se especifica en la documentación recién mencionada. Cuando se guardan objetos wandb.Image\(\) en los artefactos, hay un cambio: hemos factorizado “class\_labels”, que anteriormente necesitaba ser almacenado en cada wandb.Image.

Ahora, deberías crear las etiquetas de las clases de forma separada, si estás usando cajas delimitadoras o máscaras de segmentación, de esta forma:

```python
class_set = wandb.Classes(...)
example = wandb.Image(<path_to_image_file>, classes=class_set, masks={
            "ground_truth": {"path": <path_to_mask>}})
```

También puedes construir un wandb.Image que se refiera a un wandb.Image que haya sido registrado en un artefacto diferente. Esto va a utilizar una referencia cruzada al archivo del artefacto, evitando duplicar la imagen subyacente.

```python
artifact = wandb.use_artifact('my-dataset-1:v1')
dataset_image = artifact.get('an-image')  # if you've logged a wandb.Image here 
predicted_image = wandb.Image(dataset_image, classes=class_set, masks={
            "predictions": {"path": <path_to_mask>}})
```

  
`wandb.Classes`

Usado para definir un mapeo desde el id de una clase \(un número\) a una etiqueta \(un string\):

```python
CLASSES = ['dog', 'cat']
class_set = wandb.Classes([{'name': c, 'id': i} for i, c in enumerate(CLASSES)])
```



`wandb.JoinedTable`

Usado para decirle a la interfaz de usuario de W&B que represente la unión de dos tablas. Las tablas pueden estar almacenadas en otros artefactos.

```python
jt = wandb.JoinedTable(table1, table2, 'id')
artifact.add(jt, 'joined')
```

## Ejemplos de principio a fin

Prueba nuestra [notebook de colab](http://wandb.me/dsviz-demo-colab) para ver el ejemplo de principio a fin que cubre:

* construcciones y visualización del conjunto de datos
* entrenamiento del modelo
* registro y visualización de las predicciones contra el conjunto de datos

## Preguntas Frecuentes

**Para el entrenamiento, empaqueto mi conjunto de datos en un formato binario. ¿Cómo se relaciona esto con los Artefactos del Conjunto de Datos de W&B?**

Aquí hay algunos enfoques que puedes tomar:

1. Utiliza el formato de wandb.Table como el sistema de registro para tus conjuntos de datos. A partir de aquí, puedes hacer una de dos cosas:
   1. en tiempo de entrenamiento, deriva un formato empaquetado desde el artefacto de formatos de W&B.
   2. O, realiza un paso dentro del pipeline que produzca un artefacto con un formato empaquetado, y entrena a partir de dicho artefacto
2. Almacena tu formato empaquetado y a wandb.Table en el mismo artefacto.
3. Haz el trabajo para que, dado tu formato empaquetado, registre un artefacto wandb.Table

\[nota: en la fase Alfa, hay un límite de 50.000 filas en las tablas guardadas a los artefactos de W&B\].Si deseas consultar y visualizar las predicciones del modelo, necesitas considerar cómo pasar los IDs del ejemplo a través de tu paso de entrenamiento, para que de esta forma tu tabla de predicciones pueda volverse a unir a la tabla del conjunto de datos original. Mira nuestros ejemplos enlazados para ver algunos enfoques.Con el tiempo, vamos a proveer conversores para los formatos comunes, muchos más ejemplos, e integraciones más profundas con los frameworks populares.

##  Limitaciones actuales

Actualmente, esta característica está en una fase de acceso anticipado. Puedes usarla en nuestro servicio de producción en wandb.ai, con algunas limitaciones. La API está sujeta a cambios. ¡Si tienes alguna pregunta, comentario o idea, nos gustaría hablar! Escríbenos a [feedback@wandb.com](mailto:feedback@wandb.com).

* Escala: las tablas registradas en los artefactos en la actualidad están limitadas a 50.000 filas. Vamos a incrementar este número en cada nueva entrega, con el objetivo en mente de manejar más de 100 millones de filas por tabla en el futuro.
* Tipos de medios actualmente soportados por wandb.\*:
  * wandb.Table
  * wandb.Image
* No hay forma de guardar y persistir las consultas y las vistas en la interfaz de usuario de W&B
* No hay forma de agregar visualizaciones a los reportes de W&B

## Trabajo Futuro

* Un montón de mejoras respecto a la experiencia de usuario y a la interfaz de usuario actuales.
* Incremento del límite de filas por tabla..
*  Uso de un formato binario de columnas \(parquet\) para el almacenamiento de las tablas.. 
* Manejo de dataframes y otros otros formatos de tabla comunes en python, en adición a wandb.Table.
* Agregar un sistema de consultas más poderoso, soportando agregación y análisis más profundos..
* Soporte para más tipos de medios.. 
* Agregar la capacidad de persistir el estado de la interfaz de usuario a través del guardado de vistas / entornos de trabajo.. 
* Capacidad de guardar visualizaciones y análisis a los reportes de W&B, para compartir con los colegas.. 
* Capacidad de guardar consultas y subconjuntos para reetiquetar, y otros procesos de trabajo.. 
* Capacidad de consultar desde Python.. 
* Capacidad de agregar otras visualizaciones que utilicen datos de tablas grandes \(como un visualizador de clusters\). 
* Soporte para paneles diseñados por el usuario, para obtener visualizaciones completamente personalizadas.

