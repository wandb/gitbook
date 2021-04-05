---
description: >-
  Sintaxis para establecer los rangos de los hiperparámetros, la estrategia de
  búsqueda y otros aspectos de tu barrido.
---

# Configuration

Utiliza estos campos de configuración para personalizar tu barrido. Hay dos formas de especificar tu configuración:

1.  [Archivo YAML](https://docs.wandb.com/sweeps/quickstart#2-sweep-config): es mejor para barridos distribuidos. Mira los ejemplos [aquí](https://github.com/wandb/examples/tree/master/examples/keras/keras-cnn-fashion).
2. [Estructura de datos de Python](https://docs.wandb.ai/sweeps/python-api): es mejor para correr un barrido desde una Notebook de Jupyter.

| Subclave de la métrica | Significado |
| :--- | :--- |
| name | El nombre del barrido, visualizado en la interfaz de usuario de W&B |
| description | Descripción textual del barrido \(notas\) |
| program | Script de entrenamiento a ejecutar \(requerido\) |
| metric | Especifica la métrica a optimizar \(usado por algunas estrategias de búsqueda y por algunos criterios de detención\) |
| method | Especifica la [estrategia de búsqueda](https://docs.wandb.ai/sweeps/configuration#search-strategy) \(requerido\) |
| early\_terminate | Especifica los [criterio](https://docs.wandb.ai/sweeps/configuration#stopping-criteria)[s](https://docs.wandb.ai/sweeps/configuration#stopping-criteria)[ de detención](https://docs.wandb.ai/sweeps/configuration#stopping-criteria) \(opcional, por defecto a ninguna detención temprana\) |
| parameters | Especifica los [parámetros](https://docs.wandb.ai/sweeps/configuration#parameters) vinculados a la búsqueda \(requerido\) |
| project | Especifica el proyecto para este barrido |
| entity | Especifica la entidad para este barrido |
| command | Especifica la [línea de comandos](https://docs.wandb.ai/sweeps/configuration#command) para establecer cómo debería ser ejecutado el script de entrenamiento |

### Métrica

Especifica la métrica que hay que optimizar. Esta métrica debería ser registrada explícitamente en W&B por tu script de entrenamiento. Por ejemplo, si deseas minimizar la pérdida de validación de tu modelo:

```python
# [model training code that returns validation loss as valid_loss]
wandb.log({"val_loss" : valid_loss})
```

| Subclave de la métrica | Significado |
| :--- | :--- |
| name | Nombre de la métrica que hay que optimizar |
| goal |  `minimize` o maximize \(el valor por defecto es `minimize`\) |
| target | Valor objetivo para la métrica que estás optimizando. Cuando cualquier ejecución en el barrido consigue ese valor objetivo, el estado del barrido va a ser establecido a **finalizado**. Esto significa que todos los agentes con ejecuciones activas terminarán aquellos trabajos, pero no va a ser lanzada ninguna ejecución nueva en el barrido. |

 ⚠️ **No se pueden optimizar las métricas anidadas.**

La métrica que estás optimizando tiene que ser una métrica de **nivel superior** en la configuración.

Esto **NO** va a funcionar:  
Configuración del barrido  
`metric:   
    name: my_metric.nested`   
Código  
__`nested_metrics = {"nested": 4} wandb.log({"my_metric", nested_metrics}`

Método alternativo: registra la métrica en el nivel superior

Configuración del barrido  
`metric:   
    name: my_metric_nested`   
Código`nested_metrics = {"nested": 4} wandb.log{{"my_metric", nested_metric} wandb.log({"my_metric_nested", nested_metric["nested"]})`



**Ejemplos**

{% tabs %}
{% tab title="Maximizar" %}
```text
metric:
  name: val_loss
  goal: maximize
```
{% endtab %}

{% tab title="Minimizar " %}
```text
metric:
  name: val_loss
```
{% endtab %}

{% tab title="Objetivo" %}
```text
metric:
  name: val_loss
  goal: maximize
  target: 0.1
```
{% endtab %}
{% endtabs %}

### Estrategia de Búsqueda

 Especifica la estrategia de búsqueda con la clave `method` en la configuración del barrido

| `method` | Significado |
| :--- | :--- |
| grid | La búsqueda en rejilla itera sobre todas las posibles combinaciones de valores de los parámetros. |
| random | La búsqueda aleatoria elige conjuntos de valores aleatorios. |
| bayes | La Optimización Bayesiana utiliza un proceso gaussiano para modelar la función, y entonces elige los parámetros para optimizar la probabilidad de la mejora. Esta estrategia requiere que sea especificada una clave de la métrica. |

**Ejemplos**

{% tabs %}
{% tab title="Random search" %}
```text
method: random
```
{% endtab %}

{% tab title="Grid search" %}
```text
method: grid
```
{% endtab %}

{% tab title="Bayes search" %}
```text
method: bayes
metric:
  name: val_loss
  goal: minimize
```
{% endtab %}
{% endtabs %}

### Criterios de Detención

La detención temprana es una característica optativa que acelera la búsqueda de los hiperparámetros, al detener las ejecuciones con un desempeño pobre. Cuando se dispara una detención temprana, el agente detiene la ejecución actual y obtiene el próximo conjunto de hipreparámetros a probar.

| Subclave de early\_terminate | Significado |
| :--- | :--- |
| type | especifica el algoritmo de detención |

Soportamos el\(los\) siguiente\(s\) algoritmo\(s\) de detención:

| `type` | Significado |
| :--- | :--- |
| hyperband | Utiliza el [método hyperband](https://arxiv.org/abs/1603.06560) |

La detención hyperband evalúa si un programa debería ser detenido o si hay que permitirle continuar por uno o más tramos durante la ejecución del programa. Los tramos son configurados en iteraciones estáticas para una métrica especificada \(en donde un iteración es el número de veces que una métrica ha sido registrada – si la métrica es registrada en cada época, entonces hay un número de épocas igual al de las iteraciones\).

Para especificar la planificación de los tramos, es necesario definir a `min_iter` o a `max_iter`

| `early_terminate` sub-key | Significado |
| :--- | :--- |
| min\_iter | especifica la iteración para el primer tramo |
| max\_iter | especifica el número máximo de iteraciones para el programa |
| s | especifica el número total de tramos \(requerido `para max_iter`\) |
| eta | especifica la planificación multiplicadora de tramos \(por defecto: 3\) |

**Ejemplos**

{% tabs %}
{% tab title="Hyperband \(min\_iter\)" %}
```text
early_terminate:
  type: hyperband
  min_iter: 3
```

Brackets: 3, 9 \(3\*eta\), 27 \(9 \* eta\), 81 \(27 \* eta\)
{% endtab %}

{% tab title="Hyperband \(max\_iter\)" %}
```text
early_terminate:
  type: hyperband
  max_iter: 27
  s: 2
```

Brackets: 9 \(27/eta\), 3 \(9/eta\)
{% endtab %}
{% endtabs %}

### Parámetros

Describe the hyperparameters to explore. For each hyperparameter, specify the name and the possible values as a list of constants \(for any method\) or specify a distribution \(for `random` or `bayes` \).

| Values | Meaning |
| :--- | :--- |
| values: \[\(type1\), \(type2\), ...\] | Especifica todos los valores válidos para este hiperparámetro. Compatible con `grid`. |
| value: \(type\) | Especifica el valor válido simple para este hiperparámetro. Compatible con `grid`. |
| distribution: \(distribution\) | Selecciona una distribución desde la tabla de distribuciones de abajo. Si no está especificado, el valor por defecto será categorical si values está establecido, `int_uniform` si max y min son establecidos como enteros, uniform si `max` y `mix` son establecidos como floats, o constant si value está establecido. |
| min: \(float\) max: \(float\) | Valores máximo y mínimo válidos para `uniform` – hiperparámetros distribuidos. |
| min: \(int\) max: \(int\) | Valores máximo y mínimo para int\_uniform – hiperparámetros distribuidos. |
| mu: \(float\) | Parámetro promedio para normal – o lognormal – hiperparámetros distribuidos. |
| sigma: \(float\) | Parámetro de la desviación estándar para normal – o lognormal - hiperparámetros distribuidos. |
| q: \(float\) | Tamaño del paso de cuantificación para los hiperparámetros cuantificados. |

 **Ejemplo**

{% tabs %}
{% tab title="grid - single value" %}
```text
parameter_name:
  value: 1.618
```
{% endtab %}

{% tab title="grid - multiple values" %}
```text
parameter_name:
  values:
  - 8
  - 6
  - 7
  - 5
  - 3
  - 0
  - 9
```
{% endtab %}

{% tab title="random or bayes - normal distribution" %}
```text
parameter_name:
  distribution: normal
  mu: 100
  sigma: 10
```
{% endtab %}
{% endtabs %}

Distribuciones

| Nombre | Significado |
| :--- | :--- |
| constant | Distribución constante. Debe especificar `value`. |
| categorical | Distribución categórica. Debe especificar `values`. |
| int\_uniform | Distribución uniforme discreta sobre los enteros. Debe especificar a `max` y a `min` como enteros.  |
| uniform | Continuous uniform distribution. Must specify `max` and `min` as floats. Distribución uniforme continua. Debe especificar a max y a min como floats. |
| q\_uniform | Distribución uniforme cuantificada. Devuelve round`(X / q) * q`, en donde X es uniforme. El valor predeterminado de q es `1`. |
| log\_uniform | Distribución uniforme logarítmica. Devuelve un valor entre `exp(min)` y `exp(max)`, de tal forma que el logaritmo natural esté uniformemente distribuido entre `min` y `max`. |
| q\_log\_uniform | Distribución uniforme logarítmica cuantificada. Devuelve round\``(X / q) * q`, en donde X es `log_uniform`. El valor predeterminado de q es 1. |
| normal | Distribución normal. El valor devuelto está normalmente distribuido, con una media mu \(por defecto 0\) y una desviación estándar sigma \(por defecto `1`\). |
| q\_normal | Quantized normal distribution. Returns `round(X / q) * q` where `X` is `normal`. Q defaults to 1. |
| log\_normal | Distribución normal cuantificada. Devuelve round\(X / q\) \* q, en donde X es normal. q por defecto es 1. |
| q\_log\_normal | Quantized log normal distribution. Returns `round(X / q) * q` where `X` is `log_normal`. `q` defaults to `1`. |

 **Ejemplo**

{% tabs %}
{% tab title="constant" %}
```text
parameter_name:
  distribution: constant
  value: 2.71828
```
{% endtab %}

{% tab title="categorical" %}
```text
parameter_name:
  distribution: categorical
  values:
  - elu
  - celu
  - gelu
  - selu
  - relu
  - prelu
  - lrelu
  - rrelu
  - relu6
```
{% endtab %}

{% tab title="uniform" %}
```text
parameter_name:
  distribution: uniform
  min: 0
  max: 1
```
{% endtab %}

{% tab title="q\_uniform" %}
```text
parameter_name:
  distribution: q_uniform
  min: 0
  max: 256
  q: 1
```
{% endtab %}
{% endtabs %}

### Línea de Comandos <a id="command"></a>

 El agente del barrido, por defecto, construye una línea de comandos con el siguiente formato:

```text
/usr/bin/env python train.py --param1=value1 --param2=value2
```

{% hint style="info" %}
En máquinas con Windows /usr/bin/env va a ser omitido. En sistemas de tipo UNIX, se asegura de que sea seleccionado el intérprete de Python correcto, basándose en el entorno.
{% endhint %}

Esta línea de comandos puede ser modificada al especificar una clave `command` en el archivo de configuración.

Por defecto, el comando está definido como:

```text
command:
  - ${env}
  - ${interpreter}
  - ${program}
  - ${args}
```

| Macro de comandos | Expansión |
| :--- | :--- |
| ${env} | /usr/bin/env en sistemas de tipo UNIX, omitido en Windows |
| ${interpreter\| | Expande a “python”. |
| ${program} | Script de entrenamiento especificado por la clave `program` de la configuración del barrido |
| ${args} | Argumentos expandidos en la forma –param1=value1 --param2=value2 |
| ${args\_no\_hyphens} | Argumentos expandidos en la forma param1=value1 param2=value2 |
| ${json} | Argumentos codificados como JSON |
| ${json\_file} | La ruta al archivo que contiene los argumentos codificados como JSON |

 Ejemplos:

{% tabs %}
{% tab title="Establece el intérprete de Python" %}
Para hardcodear al interprete de python, puedes especificarlo de forma explícita:

```text
command:
  - ${env}
  - python3
  - ${program}
  - ${args}
```
{% endtab %}

{% tab title="Agrega parámetros extra" %}
Agrega argumentos adicionales a la línea de comandos, no especificados por los parámetros de configuración del barrido:

```text
command:
  - ${env}
  - ${interpreter}
  - ${program}
  - "-config"
  - your-training-config
  - ${args}
```
{% endtab %}

{% tab title="Omite argumentos" %}
 Si tu programa no usa el análisis de argumentos, puedes evitar por completo el pasaje de argumentos y sacar ventaja de `wandb.init()`, al seleccionar parámetros de barrido de forma automática:

```text
command:
  - ${env}
  - ${interpreter}
  - ${program}
```
{% endtab %}

{% tab title="Uso con Hydra" %}
Puedes cambiar el comando para pasar argumentos de la forma como lo esperan las herramientas como Hydra.

```text
command:
  - ${env}
  - ${interpreter}
  - ${program}
  - ${args_no_hyphens}
```
{% endtab %}
{% endtabs %}

## Preguntas Comunes

###  Configuración Anidada

Ahora mismo, los barridos no soportan los valores anidados, pero nuestro plan es soportarlos en el futuro cercano.

