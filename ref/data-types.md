---
description: wandb.data_types
---

# Data Types Reference

 [fuente](https://github.com/wandb/client/blob/master/wandb/data_types.py#L0)

Wandb tiene tipos de datos especiales para registrar visualizaciones enriquecidas.

Todos los tipos de datos especiales son subclases de WBValue. Todos los tipos de datos se serializan a JSON, puesto que esto es lo que wandb utiliza para guardar localmente a los objetos y subirlos al servidor de W&B.

## WBValue

 [fuente](https://github.com/wandb/client/blob/master/wandb/data_types.py#L43)

```python
WBValue(self)
```

Clase padre abstracta para las cosas que pueden ser registradas con wandb.log\(\) y visualizadas por wandb.

Los objetos van a ser serializados como JSON y siempre van a tener un atributo \_type que indique cómo interpretar a los otros campos.

**Devuelve:**

Una representación de este objeto en un diccionario compatible con JSON, que después puede ser serializado a un string.

## Histogram

 [fuente](https://github.com/wandb/client/blob/master/wandb/data_types.py#L64)

```python
Histogram(self, sequence=None, np_histogram=None, num_bins=64)
```

Clase de wandb para los histogramas.

Este objeto funciona de la misma forma que la función histogram de numpy [https://docs.scipy.org/doc/numpy/reference/generated/numpy.histogram.html](https://docs.scipy.org/doc/numpy/reference/generated/numpy.histogram.html)

**Ejemplos:**

Genera un histograma a partir una secuencia.

```python
wandb.Histogram([1,2,3])
```

Inicializa eficientemente desde np.histogram.

```python
hist = np.histogram(data)
wandb.Histogram(np_histogram=hist)
```

#### Argumentos:

* `sequence` de tipo arreglo – datos de entrada para el histograma
*  `np_histogram` histograma de numpy – entrada alternativa de un histograma precomputado
* `num_bins int` – Número de contenedores para el histograma. El número de contenedores por defecto es 64. El número máximo de contenedores es 512.

  
**Atributos:**

* bins \[float\] – límites de los contenedores
* histogram \[int\] – número de elementos que caen dentro de cada contenedor

## Media

 [fuente](https://github.com/wandb/client/blob/master/wandb/data_types.py#L122)

```python
Media(self, caption=None)
```

Un WBValue que almacenamos como un archivo fuera de JSON y que se muestra en el panel de medios en el fronted.

Si es necesario, movemos o copiamos el archivo en el directorio de medios de Run, para que éste sea subido.

## BatchableMedia

 [fuente](https://github.com/wandb/client/blob/master/wandb/data_types.py#L232)

```python
BatchableMedia(self, caption=None)
```

Clase padre para Media con la que tratamos especialmente en los lotes, como imágenes o imágenes en miniatura.

Además de las imágenes, usamos estos lotes para ayudar a organizar los archivos por nombre en el directorio de medios.

## Table

[fuente](https://github.com/wandb/client/blob/master/wandb/data_types.py#L244)

```python
Table(self,
      columns=['Input', 'Output', 'Expected'],
      data=None,
      rows=None,
      dataframe=None)
```

 Esta es una tabla diseñada para visualizar pequeños conjuntos de registros.

 **Argumentos:**

* `columns` \[str\] – Nombres de las columnas en la tabla. Los valores por defecto son \[“Input”, “Output”, “Expected”\].
* `data arreglo` – Arreglo en 2D de los valores que van a ser mostrados como strings.
* `dataframe` pandas.DataFrame – Objeto DataFrame utilizado para crear la tabla. Cuando está establecido, los otros argumentos son ignorados.

## Audio

 [fuente](https://github.com/wandb/client/blob/master/wandb/data_types.py#L305)

```python
Audio(self, data_or_path, sample_rate=None, caption=None)
```

Clase de wandb para los clips de audio.

**Argumentos**:

* `data_or_path` string o arreglo numpy – Una ruta a un archivo de audio o un arreglo numpy de los datos del audio.
* `sample_rate int` – Frecuencia de muestreo, es requerida cuando se pasa un arreglo numpy puro de los datos del audio.
* `caption string` – Leyenda que se va a mostrar con el audio.

## Object3D

 [fuente](https://github.com/wandb/client/blob/master/wandb/data_types.py#L404)

```python
Object3D(self, data_or_path, **kwargs)
```

Clase de wandb para las nubes de puntos en 3D.

**Argumentos**:

data\_or\_path \(arreglo numpy \| string \| io\): Objetc3D puede ser inicializado desde un archivo o desde un arreglo numpy.

Los tipos de archivos soportados son obj, gltf, babylon, stl. Puedes pasar una ruta a un archivo, o un objeto io y un file\_type que tiene que ser uno de los siguientes `'obj', 'gltf', 'babylon', 'stl'.`

La forma del arreglo numpy debe ser así:

```python
[[x y z],       ...] nx3
[x y z c],     ...] nx4 where c is a category with supported range [1, 14]
[x y z r g b], ...] nx4 where is rgb is color
```

## Molecule

 [fuente](https://github.com/wandb/client/blob/master/wandb/data_types.py#L527)

```python
Molecule(self, data_or_path, **kwargs)
```

 Clase de wandb para los datos moleculares

**Argumentos**:

data\_or\_path \( string \| io \): Molecule puede ser inicializado a partir del nombre de un archivo o a partir de un objeto io.

## Html

 [fuente](https://github.com/wandb/client/blob/master/wandb/data_types.py#L611)

```python
Html(self, data, inject=True)
```

Clase de wandb para un html arbitrario

**Argumentos:**

* `data string` u objeto io – HTML a visualizar en wandb
* `inject booleano` – Agrega una hoja de estilo al objeto HTML. Si está establecido a False, el HTML se va a pasar sin cambios.

## Video

 [fuente](https://github.com/wandb/client/blob/master/wandb/data_types.py#L680)

```python
Video(self, data_or_path, caption=None, fps=4, format=None)
```

Representación de wandb del video.

 **Argumentos:**

data\_or\_path \(arreglo numpy \| string \| io\): El video puede ser inicializado con una ruta a un archivo o con un objeto io. El formato debe ser "gif", "mp4", "webm" o "ogg". El formato debe ser especificado con el argumento fomrat. El video puede ser inicializado con un tensor numpy. El tensor numpy debe ser de 4 o 5 dimensiones. Los canales deberían ser \(tiempo, canal, alto, ancho\) o \(lote, tiempo, canal, alto, ancho\).

* `caption string` – leyenda que se va a mostrar con el video
* `fps int` – marcos por segundo para el video. El valor predeterminado es 4.
* `format string` – formato del video, es necesario si se inicializa con una ruta o con un objeto io.

## Image

[fuente](https://github.com/wandb/client/blob/master/wandb/data_types.py#L827)

```python
Image(self,
      data_or_path,
      mode=None,
      caption=None,
      grouping=None,
      boxes=None,
      masks=None)
```

Clase de wandb para las imágenes.

**Argumentos**:

* `data_or_path` arreglo numpy \| string \| io – Acepta un arreglo numpy de los datos de la imagen, o una imagen PIL. Esta clase intenta inferir el formato de los datos y los convierte.
* `mode string` – El modo PIL para una imagen. Los más comunes son “L”, “RGB”, “RGBA”. Una explicación completa puede ser encontrada en [https://pillow.readthedocs.io/en/4.2.x/handbook/concepts.html\#concept-modes](https://pillow.readthedocs.io/en/4.2.x/handbook/concepts.html#concept-modes).
* `caption string` – Etiqueta para mostrar con la imagen.

## JSONMetadata[ fuente](https://github.com/wandb/client/blob/master/wandb/data_types.py#L1093)

```python
JSONMetadata(self, val, **kwargs)
```

JSONMetadata es un tipo para codificar metadatos arbitrarios como archivos.

## BoundingBoxes2D

 [fuente](https://github.com/wandb/client/blob/master/wandb/data_types.py#L1126)

```python
BoundingBoxes2D(self, val, key, **kwargs)
```

Clase de wandb para cajas delimitadoras en 2D

## ImageMask

 [fuente](https://github.com/wandb/client/blob/master/wandb/data_types.py#L1204)

```python
ImageMask(self, val, key, **kwargs)
```

Clase de wandb para las máscaras de las imágenes, es útil para las tareas de segmentación.

## Plotly

 [fuente](https://github.com/wandb/client/blob/master/wandb/data_types.py#L1274)

```python
Plotly(self, val, **kwargs)
```

Clase de wandb para los gráficos plotly

**Arguments**:

* `val` - figura de matplotlib o de plotly

## Graph

 [fuente](https://github.com/wandb/client/blob/master/wandb/data_types.py#L1314)

```python
Graph(self, format='keras')
```

Clase de wandb para los gráficos.

Esta clase típicamente es utilizada para guardar y visualizar modelos de redes neuronales. Representa al gráfico como a un arreglo de nodos y aristas. Los nodos pueden tener etiquetas que pueden ser visualizadas por wandb.

**Ejemplos:**

Importa un modelo de keras:

```python
Graph.from_keras(keras_model)
```

 **Atributos:**

*  `format string` – Formato para ayudar a que wandb visualice el gráfico elegantemente.
* `nodes` \[wandb.Node\] – Lista de wandb.Nodes
* `nodes_by_id diccionario` – diccionario de ids → aristas de los nodos \(\[\(wandb.Node, wandb.Node\)\]\): Lista de pares de nodos interpretados como aristas
*  `loaded booleano`: Bandera para determinar si el gráfico está completamente cargado
*  `root wandb.Node` – Nodo raíz del gráfico.

## Node

 [fuente](https://github.com/wandb/client/blob/master/wandb/data_types.py#L1470)

```python
Node(self,
     id=None,
     name=None,
     class_name=None,
     size=None,
     parameters=None,
     output_shape=None,
     is_output=None,
     num_parameters=None,
     node=None)
```

Nodo usado en [`Graph`](https://docs.wandb.ai/ref/data-types#graph)\`\`

## Edge

 [fuente](https://github.com/wandb/client/blob/master/wandb/data_types.py#L1636)

```python
Edge(self, from_node, to_node)
```

Arista usada en [`Graph`](https://docs.wandb.ai/ref/data-types#graph)\`\`

