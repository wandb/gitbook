---
description: >-
  Instruye de forma sencilla a tu script para ver nuestras características de
  seguimiento y visualización de experimentos en tu propio proyecto
---

# Quickstart

Comienza los experimentos de aprendizaje de máquinas en tres simples pasos.

## 1. Instala la Biblioteca

 Instala nuestra biblioteca en un entorno que use Python 3.

```bash
pip install wandb
```

{% hint style="info" %}
Si estás entrenando modelos en un entorno automatizado donde sea inapropiado correr comandos del shell, tal como CloudML de Google, deberías echar un vistazo a nuestras [Variables de Entorno.](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MSSO0OxJem4ciGbHImo/v/espanol/library/environment-variables)
{% endhint %}

## 2. Crea una Cuenta

Registra una cuenta gratuita en tu shell o ve a nuestra [página de registro](https://wandb.ai/login?signup=true).

```bash
wandb login
```

## 3. Modifica tu script de entrenamiento

Agrega algunas líneas a tu script para registrar hiperparámetros y métricas.

{% hint style="info" %}
Weights and Biases es un framework agnóstico, pero si estás usando un framework de ML común, puedes encontrar que los ejemplos específicos al framework son incluso más fáciles para empezar. Hemos construido enganches específicos al framework para simplificar la integración para  [Keras](integrations/keras.md), [TensorFlow](integrations/tensorflow.md), [PyTorch](integrations/pytorch.md), [Fast.ai](integrations/fastai/), [Scikit](integrations/scikit.md), [XGBoost](integrations/xgboost.md), [Catalyst](integrations/catalyst.md)
{% endhint %}

###  Inicializar W&B

Inicializa `wandb` al comienzo de tu script, antes de comenzar el registro. Algunas integraciones, como nuestra integración con [Hugging Face](integrations/huggingface.md) , incluyen internamente wandb.init\(\).

```python
# Inside my model training code
import wandb
wandb.init(project="my-project")
```

 Creamos automáticamente el proyecto si este no existe. Las ejecuciones del script de entrenamiento anterior se sincronizarán con un proyecto llamado “my-project”. Mira la documentación de [wandb.init](library/init.md) para obtener más opciones de inicialización.

###  Declarar Hiperparámetros

Es fácil guardar hiperparámetros con el objeto [wandb.config](library/config.md) .

```python
wandb.config.dropout = 0.2
wandb.config.hidden_layer_size = 128
```

###  Registra Métricas

Registra métricas, como pérdida o precisión, mientras tu modelo se entrena \(en muchos casos, proveemos opciones por defecto específicas al framework\). Registra salidas más complicadas como histogramas, gráficos, o imágenes con [wandb.log](library/log.md).

```python
def my_train_loop():
    for epoch in range(10):
        loss = 0 # change as appropriate :)
        wandb.log({'epoch': epoch, 'loss': loss})
```

###  Guardar Archivos

Todo lo guardado en el directorio `wandb.run.dir` será subido a W&B y guardado con tu ejecución cuando ésta se complete. Esto es especialmente conveniente para guardar los pesos y las desviaciones literales de tu modelo:

```python
# by default, this will save to a new subfolder for files associated
# with your run, created in wandb.run.dir (which is ./wandb by default)
wandb.save("mymodel.h5")

# you can pass the full path to the Keras model API
model.save(os.path.join(wandb.run.dir, "mymodel.h5"))
```

¡Genial! Ahora corre tu script normalmente y nosotros sincronizaremos los registros en un proceso en segundo plano. La salida de tu terminal, las métricas y los archivos serán sincronizados en la nube, conjuntamente con un registro del estado de tu git, si es que lo estás corriendo desde un repositorio git.

{% hint style="info" %}
 Si estás testeando y deseas deshabilitar la sincronización de wandb, establece la variable de entorno WANDB\_MODE=dryrun
{% endhint %}

## Próximos Pasos

Ahora que has hecho funcionar la instrumentación, aquí hay un resumen simple de las mejores características:

1. **Página del Proyecto**: Compara muchos experimentos diferentes en un tablero de control de proyectos. Cada vez que corres un modelo en un proyecto, aparece una nueva línea en los gráficos y en la tabla. Haz click en el icono de la tabla, en el recuadro izquierdo, para expandir la tabla y ver todos los hiperparámetros y las métricas. Crea múltiples proyectos para organizar tus ejecuciones, y utiliza la tabla para agregar etiquetas y notas a tus ejecuciones.
2. **Visualizaciones Personalizadas**: Agrega gráficos de coordenadas paralelas, diagramas de dispersión, y otras visualizaciones avanzadas para explorar tus resultados.
3. [ **Reportes**](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MSSO0OxJem4ciGbHImo/v/espanol/reports): Agrega un panel Markdown para describir los resultados de tu investigación, conjuntamente con tus gráficos y tablas en tiempo real. ¡Los reportes facilitan compartir una captura instantánea de tu proyecto con los colaboradores, con tu profesor, o con tu jefe!
4.  [**Integraciones**](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MSSO0OxJem4ciGbHImo/v/espanol/integrations)**:** Tenemos integraciones especiales para frameworks populares como PyTorch , Keras, y XGBoost.
5. **Demostración: ¿**Estás interesado en compartir tu investigación? Siempre estamos trabajando en artículos de blog para destacar el maravilloso trabajo de nuestra comunidad. Mándanos un mensaje a [contact@wandb.com](mailto:contact@wandb.com).

###  [Contáctanos con preguntas →](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MSSO0OxJem4ciGbHImo/v/espanol/company/getting-help)

###  [Mira nuestro caso de estudio OpenAI →](https://bit.ly/wandb-learning-dexterity)

![](.gitbook/assets/image%20%2891%29.png)

