# Init

​[wandb.init\(\)](https://docs.wandb.ai/ref/init) indica el comienzo de una nueva ejecución. En un pipeline de entrenamiento de ML, podrías agregar wandb.init\(\) al comienzo de tu script de entrenamiento, así también como al comienzo de tu script de evaluación, y cada paso sería rastreado como una ejecución en W&B.

## init <a id="init"></a>

​[​![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_init.py#L499-L723)[Ver el código fuente en GitHub](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_init.py#L499-L723)​

```text
wandb.init(    job_type: Optional[str] = None,    dir=None,    config: Union[Dict, str, None] = None,    project: Optional[str] = None,    entity: Optional[str] = None,    reinit: bool = None,    tags: Optional[Sequence] = None,    group: Optional[str] = None,    name: Optional[str] = None,    notes: Optional[str] = None,    magic: Union[dict, str, bool] = None,    config_exclude_keys=None,    config_include_keys=None,    anonymous: Optional[str] = None,    mode: Optional[str] = None,    allow_val_change: Optional[bool] = None,    resume: Optional[Union[bool, str]] = None,    force: Optional[bool] = None,    tensorboard=None,    sync_tensorboard=None,    monitor_gym=None,    save_code=None,    id=None,    settings: Union[Settings, Dict[str, Any], None] = None) -> Union[Run, Dummy, None]
```

Comienza una nueva ejecución rastreada con [wandb.init\(\)](https://docs.wandb.ai/ref/init). En un pipeline de entrenamiento de ML, podrías agregar [wandb.init\(\)](https://docs.wandb.ai/ref/init) al comienzo de tu script de entrenamiento, así también como al comienzo de tu script de evaluación, y cada pieza sería rastreada como una ejecución en W&B.

​[`wandb.init()`](https://docs.wandb.ai/ref/init) produce un nuevo proceso en segundo plano para registrar datos de una ejecución, y también, por defecto, sincroniza los datos con wandb.ai para que puedas ver las visualizaciones en tiempo real. Llama a [`wandb.init()`](https://docs.wandb.ai/ref/init) para empezar una ejecución antes de registrar los datos con [`wandb.log()`](https://docs.wandb.ai/library/log)`.`

​[`wandb.init()`](https://docs.wandb.ai/ref/init) devuelve un objeto run, y también puedes acceder al objeto run con wandb.run.

| Argumentos | ​ |
| :--- | :--- |
|  `project` |  \(str, optional\) El nombre del proyecto donde estás enviando la nueva ejecución. Si el proyecto no está especificado, la ejecución se va a poner en un proyecto “No categorizado”. |
|  `entity` |  \(str, optional\) Una entidad es el nombre de usuario o el nombre de un equipo donde estás enviando las ejecuciones. Esta entidad debe existir antes de que puedas enviarle las ejecuciones, así que asegúrate de crear tu cuenta, o la de tu equipo, desde la interfaz de usuario, antes de registrar las ejecuciones. Si no especificas una entidad, la ejecución se va a enviar a la entidad por defecto, que por lo general es tu nombre de usuario. Cambia tu entidad por defecto en [Ajustes](https://wandb.ai/settings), debajo de “ubicación por defecto para crear nuevos proyectos”. |
|  `config` |  \(diccionario, argparse, absl.flags, srt, optional\) Esto establece a wandb.config, un objeto de tipo diccionario para guardar las entradas para tu trabajo, como los hiperparámetros para un modelo o los ajustes para un trabajo de procesamiento de datos. La configuración se va a mostrar en una tabla, en la interfaz de usuario, que puedes usar para agrupar, filtrar y ordenar a las ejecuciones. Las claves no deberían contener . en sus nombres, y los valores deberían estar por debajo de los 10 MB. Si existe el diccionario, argparse o absl.flags: se van a cargar los pares clave-valor en el objeto wandb.config. Si existe str: se va a buscar un archivo yaml por ese nombre, y se va a cargar la configuración desde ese archivo al objeto wandb.config. |
|  `save_code` | \(booleano, optional\) Activa esto para guardar al script principal o la notebook en W&B. Es valioso para mejorar la reproducibilidad del experimento, y para aplicar los diff al código de los experimentos desde la interfaz de usuario. Por defecto, está desactivado, pero puedes cambiar dicho comportamiento en [Ajustes](https://wandb.ai/settings). |
|  `group` | \(str, optional\) Especifica un grupo para organizar las ejecuciones individuales en un experimento más grande. Por ejemplo, puede que estés haciendo validación cruzada o que tengas múltiples trabajos que entrenan y evalúan a un modelo contra diferentes conjuntos de test. El grupo te da una forma de organizar las ejecuciones en un conjunto más grande, y puedes activarlo o desactivarlo desde la interfaz de usuario. Para más detalles, mira [Agrupamiento](https://docs.wandb.ai/library/grouping). |
|  `job_type` | \(str, optional\) Especifica el tipo de ejecución, lo cual es útil cuando estás agrupando ejecuciones en experimentos más grandes, al utilizar un grupo. Por ejemplo, podrías tener múltiples trabajos en un grupo, con tipos de trabajos como entrenamiento y evaluación. Establecer esto facilita el filtrado y el agrupamiento de las ejecuciones similares en la interfaz de usuario, así puedes comparar manzanas con manzanas. |
|  `tags` | \(list, optional\) Una lista de strings, que va a rellenar la lista de etiquetas sobre esta ejecución en la interfaz de usuario. Las etiquetas son útiles para organizar las ejecuciones de manera conjunta, o para aplicar etiquetas temporales como “referencia” o “producción”. Es fácil agregar o remover etiquetas desde la interfaz de usuario, o filtrar sólo las ejecuciones con una etiqueta específica. |
|  `name` |  \(str, optional\) Un nombre corto de presentación para esta ejecución, que es cómo vas a identificar a dicha ejecución en la interfaz de usuario. Por defecto, generamos un nombre aleatorio de dos palabras que te permite hacer fácilmente referencias cruzadas de las ejecuciones, desde la tabla a los gráficos. Mantener estos nombres cortos hace que sea más fácil leer las leyendas de los gráficos y a las tablas. Si estás buscando un lugar para guardar tus hiperparámetros, te recomendamos que lo hagas en config. |
|  `notes` | \(str, optional\) Una descripción más larga de la ejecución, como un mensaje -m commit en git. Te ayuda a recordar lo que estuviste haciendo cuando corriste esta ejecución. |
|  `dir` |  \(str, optional\) Una ruta absoluta a un directorio donde van a ser almacenados los metadatos. Cuando llamas a download\(\) sobre un artefacto, este va a ser el directorio donde se van a guardar los archivos descargados. Por defecto, es el directorio ./wandb. |
|  `sync_tensorboard` |  \(booleano, optional\) Si hay que copiar todos los registros de TensorBoard a W&B \(por defecto: False\). [Tensorboard](https://docs.wandb.com/integrations/tensorboard)​ |
|  `resume` | \(booelano str, optional\) Establece el comportamiento de reanudación. Opciones: “allow”, “must”, “never”, “auto” o None. El valor predeterminado es None. Casos: - None \(por defecto\): si la nueva ejecución tiene el mismo ID que la previa, esta ejecución sobrescribe esos datos. - “auto” \(o True\): si falló la ejecución previa sobre esta máquina, la restablece automáticamente. En caso contrario, comienza una nueva ejecución. - “allow”: Si el id es establecido con init\(id="UNIQUE\_ID"\) o con WANDB\_RUN\_ID="UNIQUE\_ID", y es idéntico al de la ejecución previa, wandb reanudará automáticamente la ejecución con dicho id. En caso contrario, wandb arrancará una nueva ejecución. - “never”:  Si el id es establecido con init\(id="UNIQUE\_ID"\) o con WANDB\_RUN\_ID="UNIQUE\_ID", y es idéntico al de la ejecución previa, wandb va a fallar. - “must”:  Si el id es establecido con init\(id="UNIQUE\_ID"\) o con WANDB\_RUN\_ID="UNIQUE\_ID", y es idéntico al de la ejecución previa, wandb va a reanudar automáticamente la ejecución con dicho id. En caso contrario, wandb va a fallar. Mira [https://docs.wandb.com/library/advanced/resuming](https://docs.wandb.com/library/advanced/resuming) para obtener más detalles. |
|  `reinit` |  \(booleano, optional\) Permite múltiples llamadas a wandb.init\(\) en el mismo proceso \(por defecto: False\) |
|  `magic` |  \(booleano, diccionario, o str, optional\) El booleano controla si intentamos instrumentar automáticamente a tu script, capturando detalles básicos de tu ejecución sin que tengas que agregar más código de wandb. \(por defecto: False\). También puedes pasar un diccionario, un string json, o un archivo yaml. |
|  `config_exclude_keys` |  \(lista, optional\) Claves de string que hay que excluir de wandb.config. |
|  `config_include_keys` | \(lista, optional\) Claves de string que hay que incluir en wandb.config. |
|  `anonymous` |  \(str, optional\) Controla el registro de datos anónimos. Opciones: - “never” \(por defecto\): requiere que enlaces tu cuenta de W&B antes de hacerle el seguimiento a la ejecución, así no creas de manera accidental una ejecución anónima. - “allow”: permite que un usuario que ha iniciado sesión, rastree las ejecuciones con su cuenta, pero permite que alguien que está corriendo el script sin una cuenta de W&B vea los gráficos en la interfaz de usuario. - “must”: envía la ejecución a una cuenta anónima, en vez de a una cuenta de un usuario registrado. |
|  `mode` | \(str, optional\): Puede ser “online”, “offline” o “disabled”. El valor predeterminado es online. |
|  `allow_val_change` |  \(booleano, optional\) Si hay que permitir que los valores de configuración cambien después de que se hayan establecido las claves. Por defecto, lanzamos una excepción si se sobrescribe un valor de configuración. Si quieres hacer el seguimiento de algo como un learning\_rate que va variando múltiples veces durante el entrenamiento, en su lugar utiliza wandb.log\(\). \(por defecto: False en los scripts, True en Jupyter\) |
|  `force` |  \(booleano, optional\) Si es True, el script falla si el usuario no ha iniciado sesión con W&B. Si es False, va a permitir que el script corra en modo offline aunque el usuario no haya iniciado sesión con W&B. \(por defecto: False\) |
|  `sync_tensorboard` | \(booleano, optional\) Sincroniza los registros de wandb desde tensorboard o tensorboardX, y guarda el archivo de los eventos relevantes. El valor predeterminado es false. |
|  `monitor_gym` |  \(booleano, optional\): Registra automáticamente videos del entorno cuando se usa OpenAI Gym. \(por defecto: False\). Mira [https://docs.wandb.com/library/integrations/openai-gym](https://docs.wandb.com/library/integrations/openai-gym) |
|  `id` |  \(str, optional\) Un ID único para esta ejecución, utilizado para la Reanudación. Debe ser único en el proyecto, y si borras una ejecución no puedes reutilizarlo. Utiliza el campo name para establecer un nombre descriptivo corto, o a config para guardar los hiperparámetros para hacer comparaciones entre las ejecuciones. Este ID no puede contener caracteres especiales. Mira [https://docs.wandb.com/library/resuming](https://docs.wandb.com/library/resuming) |

## Ejemplos: <a id="examples"></a>

Uso básico

```text
wandb.init()
```

 Lanza múltiples ejecuciones desde el mismo script

```text
for x in range(10):    with wandb.init(project="my-projo") as run:        for y in range(100):            run.log({"metric": x+y})
```

| Lanza | ​ |
| :--- | :--- |
|  `Exception` |  si hay un problema. |

| Devulve |
| :--- |
| Un objeto `Run`. |

