---
description: >-
  Agrupa ejecuciones de entrenamiento y de evaluación en experimentos más
  grandes
---

# Grouping

Agrupa ejecuciones individuales en experimentos al pasar un nombre de **grupo** único a **wandb.init\(\)**.

### Casos de Uso

1. **Entrenamiento distribuido**: Utiliza el agrupamiento si tus experimentos están divididos en diferentes piezas, con scripts de entrenamiento y de evaluación separados, que deberían ser vistos como partes de un todo más grande.
2. **Múltiples procesos:** Agrupa en un experimento múltiples procesos más pequeños.
3. **Validación cruzada k-fold:** Agrupa ejecuciones con diferentes semillas aleatorias para ver un experimento más grande. Aquí hay [un ejemplo](https://github.com/wandb/examples/tree/master/examples/wandb-sweeps/sweeps-cross-validation) de validación cruzada k-fold con barridos y agrupamiento.

### Cómo se ve

 Si estableces agrupamiento en tu script, agruparemos las ejecuciones por defecto en la tabla en la Interfaz de Usuario. Puedes activarlo o desactivarlo al hacer click en el botón **Group**, en la parte superior de la tabla. Aquí hay un ejemplo de agrupamiento en la página del proyecto.

* **Panel lateral:** las ejecuciones son agrupadas por número de épocas
* **Gráficos:** Cada línea representa la media del grupo, y el sombreado indica la varianza. Este comportamiento puede cambiarse en los ajustes del gráfico.

![](../.gitbook/assets/demo-grouping.png)

Hay algunas formas de utilizar agrupamientos:

**Estableciendo un grupo en tu script**

Pasa un grupo opcional y el job\_type a `wandb.init().` Por ejemplo: `wandb.init(group="experiment_1", job_type="eval")`. Group debería ser único dentro de tu proyecto  y debería ser compartido por todas las ejecuciones en el grupo. Puedes utilizar `wandb.util.generate_id()` para generar un string único de 8 caracteres, para usar en todos tus procesos – por ejemplo:

`os.environ["WANDB_RUN_GROUP"] = "experiment-" + wandb.util.generate_id()`

**Estableciendo una variable de entorno del grupo**

Utiliza `WANDB_RUN_GROUP` como una variable de entorno para especificar un grupo para tus ejecuciones. Para saber más acerca de esto, verifica nuestra documentación para [**Variables de Entorno**](https://docs.wandb.ai/library/environment-variables)**.**

 **Activando el agrupamiento en la Interfaz de Usuario**

Puedes agrupar dinámicamente por cualquier columna de la configuración. Por ejemplo, si utilizas `wandb.config` para registrar el tamaño del lote o la tasa de aprendizaje, entonces puedes agrupar dinámicamente por dichos hiperparámetros en la aplicación web.

### Desactiva el agrupamiento

Haz click en el botón grouping y limpia los campos del grupo en cualquier momento, lo que revierte la tabla y los gráficos a su estado no agrupado.

![](../.gitbook/assets/demo-no-grouping.png)

### Grouping graph settings

Click the edit button in the upper right corner of a graph and select the **Advanced** tab to change the line and shading. You can select the mean, minimum, or maximum value to for the line in each group. For the shading, you can turn off shading, show the min and max, the standard deviation, and the standard error.

![](../.gitbook/assets/demo-grouping-options-for-line-plots.gif)



