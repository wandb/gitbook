# Environment Variables

Cuando estás corriendo un script en un entorno automatizado, puedes controlar a wandb con las variables de entorno establecidas antes de que el script se ejecute, o dentro del mismo.

```bash
# This is secret and shouldn't be checked into version control
WANDB_API_KEY=$YOUR_API_KEY
# Name and notes optional
WANDB_NAME="My first run"
WANDB_NOTES="Smaller learning rate, more regularization."
```

```bash
# Only needed if you don't checkin the wandb/settings file
WANDB_ENTITY=$username
WANDB_PROJECT=$project
```

```python
# If you don't want your script to sync to the cloud
os.environ['WANDB_MODE'] = 'dryrun'
```

## Variables de Entorno Opcionales

Utiliza estas variables de entorno opcionales para hacer cosas como establecer la autenticación sobre las máquinas remotas.

| Nombre de la variable | Uso |
| :--- | :--- |
| **WANDB\_API\_KEY** | Establece la clave de la autenticación asociada con tu cuenta. Puedes encontrar tu clave en [tu página de ajustes](https://app.wandb.ai/settings). Esta debe ser establecida si wandb login no ha sido ejecutado en la máquina remota |
| **WANDB\_BASE\_URL** | Si estás usando [wandb/local](https://docs.wandb.ai/self-hosted), deberías establecer esta variable de entorno a`http://YOUR_IP:YOUR_PORT` |
| **WANDB\_NAME** | El nombre de tu ejecución en un formato legible por seres humanos. Si no está establecido, será generado aleatoriamente por ti. |
| **WANDB\_NOTES** | Notas más extensas acerca de tu ejecución. Está permitido el formato Markdown, y después puedes editarlo en la Interfaz de Usuario. |
| **WANDB\_ENTITY** | La entidad asociada con tu ejecución. Si has corrido `wandb init` en el directorio de tu script de entrenamiento, este creará un directorio llamado wandb y guardará una entidad por defecto que puede ser revisada en el control de código fuente. Si no deseas crear ese archivo, o quieres sobrescribirlo, puedes usar la variable de entorno. |
| **WANDB\_USERNAME** | El nombre de usuario de un miembro de tu equipo asociado con la ejecución. Esto puede ser usado conjuntamente con la clave de la API de la cuenta del servicio para permitir la atribución de ejecuciones automatizadas a los miembros de tu equipo. |
| **WANDB\_PROJECT** | El proyecto asociado con tu ejecución. Esto también puede ser establecido con `wandb init`, pero la variable de entorno sobrescribirá el valor. |
| **WANDB\_MODE** | Por defecto, esto está establecido a run, que guarda los resultados a wandb. Si solo quieres guardar los metadatos de tu ejecución localmente, puedes establecerlo a dryrun. |
| **WANDB\_TAGS** | Una lista separada por comas de las etiquetas que van a ser aplicadas en la ejecución. |
| **WANDB\_DIR** | Establece esto a una ruta absoluta para almacenar todos los archivos generados aquí, en vez de hacerlo en el directorio wandb que es relativo a tu script de entrenamiento. Asegúrate de que este directorio exista y de que el usuario que corre tu proceso tenga privilegios de escritura |
| **WANDB\_RESUME** | Por defecto, es establecido a never. Si se establece a auto, wandb reanudará automáticamente las ejecuciones fallidas. Si es establecido a must, fuerza la existencia de la ejecución al arrancar. Si siempre quieres generar tus propias ids únicas, establece esto a allow y establece siempre **WANDB\_RUN\_ID**. |
| **WANDB\_RUN\_ID** | La establece a un string globalmente único \(por proyecto\) que se corresponde con una ejecución simple de tu script. No debe ser de más de 64 caracteres. Todos los caracteres que no sean alfanuméricos van a ser convertidos a guiones. Esto puede ser utilizado para reanudar una ejecución existente en caso de fallas. |
| **WANDB\_IGNORE\_GLOBS** | Establece una lista de archivos globs a ignorar separada por comas. Estos archivos no serán sincronizados con la nube. |
| **WANDB\_ERROR\_REPORTING** | Establece esto a false para evitar que wandb registre errores fatales en su sistema de seguimiento de errores. |
| **WANDB\_SHOW\_RUN** | Establece esto a **true** para abrir un navegador automáticamente con la url de la ejecución, si tu sistema operativo lo permite. |
| **WANDB\_DOCKER** | Establece esto a un digest de una imagen de docker para habilitar la restitución de las ejecuciones. Esto es establecido automáticamente con el comando wandb docker. Puedes obtener el digest de una imagen al correr `wandb docker my/image/name:tag --digest` |
| **WANDB\_DISABLE\_CODE** | Establece esto a true para evitar que wandb almacene una referencia a tu código fuente |
| **WANDB\_ANONYMOUS** | Establece esto a “allow”, “never” o “must” para permitir que los usuarios creen ejecuciones anónimas con urls secretas |
| **WANDB\_CONSOLE** | Establece esto a “off” para deshabilitar el registro de las salidas a stdout/stderr. Por defecto, está en “on” en entornos que lo soportan. |
| **WANDB\_CONFIG\_PATHS** | Lista de archivos yaml separados por coma, para cargar en wandb.config. Ver [configuración](https://docs.wandb.ai/library/config#file-based-configs). |
| **WANDB\_CONFIG\_DIR** | Su valor por defecto es ~/.config/wandb, puedes sobrescribir esta ubicación con esta variable de entorno |
| **WANDB\_NOTEBOOK\_NAME** | Si estás trabajando con jupyter, puedes establecer el nombre de la notebook con esta variable. Intentamos detectarlo automáticamente. |
| **WANDB\_HOST** | Establece esto al hostname que quieras ver en la interfaz de wandb, en caso de que no quieras usar el hostname provisto por el sistema |
| **WANDB\_SILENT** |  Establece esto a true para silenciar la confirmación del registro de wandb. Si es establecido, todos los registros serán escritos en WANDB\_DIR/debug.log |
| **WANDB\_RUN\_GROUP** | Especifica el nombre del experimento para agrupar automáticamente a las ejecuciones. Ver [agrupamiento](https://docs.wandb.ai/library/grouping) para más información. |
| **WANDB\_JOB\_TYPE** | Especifica el tipo de trabajo, como “training” o “evaluation” para indicar diferentes tipos de ejecuciones. Ver [agrupamiento](https://docs.wandb.ai/library/grouping) para más información. |

## Entornos de Singularity

Si estás corriendo contenedores en [Singularity](https://singularity.lbl.gov/index.html), puedes pasar variables de entorno al anteponer SINGULARITYENV\_ a las variables anteriores. Pueden ser encontrados más detalles acerca de las variables de entorno de Singularity [aquí](https://singularity.lbl.gov/docs-environment-metadata#environment).

## Corriendo sobre AWS

Si estás ejecutando trabajos en lote en AWS, es fácil autenticar tus máquinas con tus credenciales de W&B. Obtén tu clave de la API desde tu [página de ajustes](https://app.wandb.ai/settings), y establece la variable de entorno WANDB\_API\_KEY en la [especificación del trabajo por lotes de AWS](https://docs.aws.amazon.com/batch/latest/userguide/job_definition_parameters.html#parameters).

##  Preguntas Comunes

### Ejecuciones automatizadas y cuentas de servicio

 Si tienes tests automatizados o herramientas internas que disparan el registro de las ejecuciones hacia W&B, crea una Cuenta de Servicio en la página de ajustes de tu equipo. Esto te permitirá usar una clave de la API del servicio para tus trabajos automatizados. Si deseas atribuir trabajos de la cuenta de servicio a un usuario específico, puedes usar las variables de entorno WANDB\_USER\_NAME o WANDB\_USER\_EMAIL.

![ Crea una cuenta de servicio en la p&#xE1;gina de ajustes de tu equipo para los trabajos automatizados](../.gitbook/assets/image%20%2892%29.png)

###  Las variables de entorno sobrescriben a los parámetros pasados a wandb.init\(\)?

Los argumentos pasados a `wandb.init()` tienen precedencia sobre el entorno. Podrías llamar a `wandb.init(dir=os.getenv("WANDB_DIR", my_default_override))` si deseas tener un valor por defecto distinto al valor por defecto del sistema, cuando la variable de entorno no está establecida.

### Desactiva el registro

El comando `wandb off` establece una variable de entorno, `WANDB_MODE=dryrun`. Esto evita que se sincronice cualquier tipo de dato de tu máquina con el servidor remoto de wandb. Si tienes múltiples proyectos, todos dejarán de sincronizar los datos registrados con los servidores de W&B.

Para silenciar los mensajes de advertencia:

```python
import logging
logger = logging.getLogger("wandb")
logger.setLevel(logging.WARNING)
```

### Múltiples usuarios de wandb en máquinas compartidas

 Si estás usando una máquina compartida, y otra persona es un usuario de wandb, es fácil asegurarte de que tus ejecuciones siempre sean registradas a la cuenta apropiada. Establece la [variable de entorno WANDB\_API\_KEY](https://docs.wandb.ai/library/environment-variables) para la autenticación. Si la pones en las variables de entorno de tu sistema, cuando inicies sesión tendrás todas las credenciales correctas, o puedes establecer la variable de entorno desde tu script.

Corre este comando export `WANDB_API_KEY=X`, donde X es tu clave de la API. Cuando hayas iniciado sesión, puedes encontrar la clave de la API en [wandb.ai/authorize](https://app.wandb.ai/authorize).

