# Command Line Reference

## wandb

 **Uso**

`wandb [OPTIONS] COMMAND [ARGS]...`

**Opciones**

| **Opciones** | Descripción |
| :--- | :--- |
| --version | Muestra la versión y sale. |
| --help | Muestra este mensaje y sale. |

 **Comandos**

| Comandos | Descripción |
| :--- | :--- |
| agent | Ejecuta el agente de W&B |
| artifact | Comandos para interactuar con los artefactos |
| controller | Ejecuta el controlador de barrido local de W&B |
| disabled | Deshabilita a W&B. |
| docker | docker te permite ejecutar tu código en una imagen de docker asegurando que... |
| docker-run | wrapper simple para docker run que establece el entorno de W&B... |
| enabled | Habilita a W&B |
| init | Configura un directorio con Weights and Biases |
| local | Lanza al contenedor local de W&B \(Experimental\) |
| login | Inicia Sesión en Weights & Biases |
| offline | Deshabilita la sincronización con W&B |
| online | Habilita la sincronización con W&B |
| pull | Toma archivos desde Weights & Biases |
| restore | Restituye código, configuración y el estado de docker para un ejecución |
| status | Muestra los ajustes de configuración |
| sweep | Crea un barrido |
| sync | Sube un directorio de entrenamiento fuera de línea a W&B |

## wandb agent

**Uso**

`wandb agent [OPTIONS] SWEEP_ID`

 **Resumen**

 Corre el agente de W&B

 **Opciones**

| Opciones | Descripción |
| :--- | :--- |
| -p, --project | El proyecto del barrido. |
| -e, --entity | El alcance de la entidad para el proyecto. |
| --count | El número máximo de ejecuciones para este agente. |
| --help | Muestra este mensaje y sale. |

## wandb artifact

**Uso**

`wandb artifact [OPTIONS] COMMAND [ARGS]...`

**Resumen**

 Comandos para interactuar con los artefactos

**Opciones**

| Opciones | Descripción |
| :--- | :--- |
| --help | Muestra este mensaje y sale. |

### wandb artifact get

**Uso**

`wandb artifact get [OPTIONS] PATH`

**Resumen**

Descarga un artefacto de wandb

**Options**

| Opciones | Descripciñon |
| :--- | :--- |
| --root | El directorio al que quieres descargar el artefacto |
| --type | El tipo del artefacto que estás descargando |
| --help | Muestra este mensaje y sale. |

### wandb artifact ls

**Uso**

`wandb artifact ls [OPTIONS] PATH`

**Resumen**

Lista todos los artefactos en un proyecto de wandb

**Opciones**

| Opciones | Descripción |
| :--- | :--- |
| -t, --type | El tipo de artefacto que se va a listar |
| --help | Muestra este mensaje y sale. |

### wandb artifact put

**Uso**

`wandb artifact put [OPTIONS] PATH`

**Resumen**

Sube un artefacto a wandb

 **Opciones**

| Opciones | Descripción |
| :--- | :--- |
| -n, --name | El nombre del artefacto que hay que enviar: |
| -d, --description | Una descripción de este artefacto |
| -t, --type | El tipo del artefacto |
| -a, --alias | Un alias que se va a aplicar a este artefacto |
| --help | Muestra este mensaje y sale. |

## wandb controller

**Uso**

`wandb controller [OPTIONS] SWEEP_ID`

 **Resumen**

 Ejecuta el controlador de barrido local de W&B

 **Opciones**

| Opciones | Descripción |
| :--- | :--- |
| --verbose | Exhibe una salida verbosa. |
| --help | Muestra este mensaje y sale. |

## wandb disabled

**Uso**

`wandb disabled [OPTIONS]`

 **Resumen**

Deshabilita a W&B.

**Opciones**

| Opciones | Descripción |
| :--- | :--- |
| --help | Muestra este mensaje y sale. |

## wandb docker

**Uso**

`wandb docker [OPTIONS] [DOCKER_RUN_ARGS]... [DOCKER_IMAGE]`

**Resumen** 

wandb docker te permite ejecutar tu código en una imagen de docker, que se asegura de que wandb esté configurado. Agrega las variables de entorno WANDB\_DOCKER y WANDB\_API\_KEY a tu contenedor y monta al directorio actual por defecto en /app. Puedes pasar argumentos adicionales que van a ser agregados a docker run antes de que sea declarado el nombre de la imagen, elegiremos una imagen predeterminada por ti si no has pasado ninguna:

wandb docker -v /mnt/dataset:/app/data wandb docker gcr.io/kubeflow- images-public/tensorflow-1.12.0-notebook-cpu:v0.4.0 --jupyter wandb docker wandb/deepo:keras-gpu --no-tty --cmd "python train.py –epochs=5"

Por defecto, sobrescribimos el punto de entrada para verificar la existencia de wandb y lo instalamos si no está presente. Si pasas la bandera --jupyter, nos aseguraremos de que jupyter esté instalado y arrancaremos un laboratorio de juypyter en el puerto 8888. Si detectamos a nvidia-docker en tu sistema, utilizaremos el runtime de nvidia. Si solo quieres que wandb establezca la variable de entorno a un comando docker run existente, mira el comando wandb docker-run.

**Opciones**

| Opciones | Descripción |
| :--- | :--- |
| --nvidia | / --no-nvidia Utiliza el runtime de nvidia, por defecto a nvidia si |
| nvidia-docker | está presente |
| --digest | Imprime el digest de la imagen y sale |
| --jupyter | / --no-jupyter Corre el laboratorio de jupyter en el contenedor |
| --dir | A qué directorio se monta el código en el contenedor |
| --no-dir | No monta el directorio actual |
| --shell | El shell con el cual comenzar el contenedor |
| --port | El puerto del host sobre el que se vincula a jupyter |
| --cmd | El comando que se va a correr en el contenedor |
| --no-tty | Corre el comando sin un tty |
| --help | Muestra este mensaje y sale. |

## wandb enabled

**Uso**

`wandb enabled [OPTIONS]`

 **Resumen**

 Habilita a W&B

 **Opciones**

| Opciones | Descripción |
| :--- | :--- |
| --help | Muestra este mensaje y sale. |

## wandb init

**Usage**

`wandb init [OPTIONS]`

**Resumen**

Configura un directorio con Weights and Biases

**Opciones**

| Opciones | Descripción |
| :--- | :--- |
| -p, --project | El proyecto que se va a usar. |
| -e, --entity | La entidad a la que se le va a dar alcance al proyecto. |
| --reset | Ajustes de reinicialización |
| -m, --mode | Puede ser “online”, “offline” o “disabled”. El valor predeterminado es |
| --help | Muestra este mensaje y sale. |

## wandb local

**Uso**

`wandb local [OPTIONS]`

**Resumen**

Lanza al contenedor local de W&B \(Experimental\)

**Opciones**

| Opciones | Descripción |
| :--- | :--- |
| -p, --port | El puerto del host sobre el que se vincula localmente a wandb |
| -e, --env | Variables de entorno que se pasan a wandb/local |
| --daemon | / --no-daemon Ejecuta, o no, en modo demonio |
| --upgrade | Actualiza a la versión más reciente |
| --help | Muestra este mensaje y sale. |

## wandb login

**Uso**

`wandb login [OPTIONS] [KEY]...`

 **Resumen**

Inicia Sesión en Weights & Biases

**Opciones**

|  **Opciones** | Descripción |
| :--- | :--- |
| --cloud | Inicia sesión en la nube, en vez de localmente |
| --host | Inicia sesión con una instancia específica de W&B |
| --relogin | Fuerza el reinicio de sesión en el caso en que ésta ya se haya realizado |
| --anonymously | Inicia sesión anónimamente |
| --help | Muestra este mensaje y sale. |

## wandb offline

**Uso**

`wandb offline [OPTIONS]`

 **Resumen**

Deshabilita la sincronización con W&B

**Opciones**

| Opciones | Descripción |
| :--- | :--- |
| --help | Muestra este mensaje y sale. |

## wandb online

**Uso**

`wandb online [OPTIONS]`

**Resumen**

Habilita la sincronización con W&B

**Opciones**

| Opciones | Descripción |
| :--- | :--- |
| --help | Muestra este mensaje y sale. |

## wandb pull

**Uso**

`wandb pull [OPTIONS] RUN`

 **Resumen**

Toma archivos desde Weights & Biases

**Opciones**

| Opciones | Descripción |
| :--- | :--- |
| -p, --project | El proyecto que quieres descargar |
| -e, --entity | La entidad a la que se le va a dar alcance al listado. |
| --help | Muestra este mensaje y sale. |

## wandb restore

**Uso**

`wandb restore [OPTIONS] RUN`

**Resumen**

Restituye código, configuración y el estado de docker para un ejecución

Opciones

| Opciones | Descripción |
| :--- | :--- |
| --no-git | Skupp |
| --branch | / --no-branch  Si crear una rama o un checkout detached |
| -p, --project | El proyecto al que deseas subir |
| -e, --entity | La entidad a la que se le va a dar alcance al listado. |
| --help | Muestra este mensaje y sale. |

## wandb status

**Uso**

`wandb status [OPTIONS]`

**Resumen**

Muestra los ajustes de la configuración

**Options**

| Opciones | Descripción |
| :--- | :--- |
| --settings | / --no-settings  Muestra los ajustes actuales |
| --help | Muestra este mensaje y sale. |

## wandb sweep

 **Uso**

`wandb sweep [OPTIONS] CONFIG_YAML`

**Resumen**

Crea un barrido

**Opciones**

| Opciones | Descripción |
| :--- | :--- |
| -p, --project | El proyecto del barrido. |
| -e, --entity | El alcance de la entidad para el proyecto. |
| --controller | Corre un controlador local |
| --verbose | Exhibe salida verbosa |
| --name | Establece el nombre del barrido |
| --program | Establece el programa del barrido |
| --update | Actualiza un barrido pendiente |
| --help | Muestra este mensaje y sale. |

## wandb sync

**Uso**

`wandb sync [OPTIONS] [PATH]...`

**Resumen**

 Sube un directorio de entrenamiento fuera de línea a W&B

 **Opciones**

|  ****Opciones | Descripción |
| :--- | :--- |
| --id | La ejecución a la que deseas subir. |
| -p, --project | El proyecto al que deseas subir |
| -e, --entity | La entidad a la cual darle alcance. |
| --include-globs | Lista separada por comas de los globs que se van a incluir. |
| --exclude-globs | Lista separada por comas de los globs que se van a excluir. |
| --include-online | / --no-include-online |
| Include | ejecuciones en línea |
| --include-offline | / --no-include-offline |
| Include | ejecuciones fuera de línea |
| --include-synced | / --no-include-synced |
| Include | ejecuciones sincronizadas |
| --mark-synced | / --no-mark-synced |
| Mark | las ejecuciones como sincronizadas |
| --sync-all | Sincroniza todas las ejecuciones |
| --clean | Borra las ejecuciones sincronizadas |
| --clean-old-hours | Borra las ejecuciones creadas antes de tantas horas. |
| Para | ser usado con la bandera –clean. |
| --clean-force | Limpia sin pedir la confirmación. |
| --show | Número de ejecuciones que se van a mostrar. |
| --help | Muestra este mensaje y sale. |

