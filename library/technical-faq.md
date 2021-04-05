# Technical FAQ

###  ¿Qué hace esto a mi proceso de entrenamiento?

Cuando `wandb.init()` es llamado desde tu script de entrenamiento, se hace una llamada a una API para crear un objeto de ejecución en nuestro servidores. Es iniciado un nuevo proceso para transmitir y recoger métricas, consecuentemente manteniendo todos los hilos y la lógica fuera de tu proceso principal. Tu script corre normalmente y escribe a los archivos locales, mientras que el proceso separado los transmite a nuestros servidores, junto con las métricas del sistema. Siempre puedes desactivar la transmisión al correr `wandb off` desde tu directorio de entrenamiento, o estableciendo la variable de entorno **WANDB\_MODE** a “dryrun”.

### Si wandb colapsa, ¿posiblemente colapsará mi ejecución de entrenamiento?

Es extremadamente importante para nosotros nunca interferir con tus ejecuciones de entrenamiento. Corremos wandb en un proceso separado para asegurarnos de que si wandb se rompe de alguna manera, tu entrenamiento siga corriendo. Si internet se cae, wandb seguirá intentando enviar datos a wandb.com.

### ¿Wandb va a ralentizar a mi entrenamiento?

Wandb debería tener un efecto insignificante en el desempeño del entrenamiento si lo utilizas normalmente. Un uso normal de wandb significa registrar menos de una vez por segundo y registrar menos de algunos pocos megabytes de datos en cada paso. Wandb corre en un proceso separado y las llamadas a las funciones no son bloqueantes, así que si la red se cae brevemente, o hay problemas de lectura/escritura intermitentes en el disco, esto no debería afectar tu desempeño. Es posible registrar una enorme cantidad de datos rápidamente, y si lo haces podrías crear problemas de entrada/salida en en disco. Si tienes alguna pregunta, por favor no dudes en contactarnos.

### ¿Puedo correr wandb cuando estoy desconectado?

Si estás entrenando una máquina que está fuera de línea y después quieres subir los resultados a nuestros servidores, ¡tenemos una característica para ti!

1. Establece la variable de entorno `WANDB_MODE=dryrun` para guardar las métricas localmente, no es requerido internet.
2. Cuando estés listo, corre `wandb init` en tu directorio para establecer el nombre del proyecto.
3.  Corre `wandb sync YOUR_RUN_DIRECTORY` para subir las métricas a nuestro servicio de nube y ver los resultados en nuestra aplicación web.

### ¿La herramienta hace seguimiento o almacena datos de entrenamiento?

 Puedes pasar un SHA, u otro identificador único, a `wandb.config.update(…)` para asociar un conjunto de datos con una ejecución de entrenamiento. W&B no almacena ningún dato a menos que `wandb.save` sea llamado con el nombre del archivo local.

###  ¿Con qué frecuencia son recogidas las métricas del sistema?

Por defecto, las métricas son recogidas cada 2 segundos y promediadas sobre un período de 30 segundos. Si necesitas métricas de resolución más grandes, mándanos un email a [contact@wandb.com](mailto:contact@wandb.com).

### ¿Sólo funciona con Python?

Actualmente, la biblioteca sólo funciona con proyectos de Python 2.7+ y  3.6+. La arquitectura mencionada anteriormente debería permitirnos una fácil integración con otros lenguajes. Si tienes la necesidad de monitorear otros lenguajes, envíanos una nota a [contact@wandb.com](mailto:contact@wandb.com).

###  ¿Puedo registrar sólo métricas, sin hacerlo con el código o los ejemplos de conjuntos de datos?

**Ejemplos de conjuntos de datos**

Por defecto, no registramos ninguno de los ejemplos de tus conjuntos de datos. Puedes activar explícitamente esta característica para ver las predicciones de los ejemplos en nuestra interfaz web.

 **Registro del código**

Hay dos maneras de desactivar el registro del código:

1. Establecer **WANDB\_DISABLE\_CODE** a true para desactivar todo el seguimiento del código. No vamos a recoger el SHA de git o el parche diff.
2. Establecer **WANDB\_IGNORE\_GLOBS** a \*.patch para desactivar la sincronización del parche diff con nuestros servidores. Aún lo tendrás localmente, y serás capaz de aplicarlo con el comando [wandb restore](https://docs.wandb.ai/library/cli#restore-the-state-of-your-code).

### ¿El registro bloquea mi entrenamiento?

“¿Es la función de registro perezosa? No quiero esperar a que la red envie los resultados a sus servidores y entonces continuar con mis operaciones locales.”

Al llamar a **wandb.log** se escribe una línea en un archivo local; esto no bloquea ninguna llamada de red. Cuando llamas a wandb.init, lanzamos un nuevo proceso en la misma máquina que escucha los cambios en el sistema de archivos y habla asincrónicamente con nuestro servicio web desde tu proceso de entrenamiento.

### ¿Qué fórmula utilizan para su algoritmo de alisado?

Utilizamos la misma fórmula del promedio móvil exponencial que es usada por TensorFlow. Puedes encontrar una explicación aquí: [https://stackoverflow.com/questions/42281844/what-is-the-mathematics-behind-the-smoothing-parameter-in-tensorboards-scalar](https://stackoverflow.com/questions/42281844/what-is-the-mathematics-behind-the-smoothing-parameter-in-tensorboards-scalar).

### ¿Qué tiene de diferente W&B respecto de TensorBoard?

¡Amamos a la gente de TensorBoard, y tenemos una [integración con TensorBoard](https://docs.wandb.ai/integrations/tensorboard)! Nos inspiramos en ellos para mejorar las herramientas de seguimiento de experimentos para todo el mundo. Cuando los cofundadores comenzaron a trabajar en W&B, se inspiraron en construir una herramienta para los usuarios insatisfechos de TensorBoard en OpenAI. Aquí hay algunas cosas en las que nos enfocamos para mejorar:

1. **Reproducir modelos:** Weights & Biases es bueno para la experimentación, la exploración, y la reproducción de modelos. No solo que capturamos las métricas, sino también los hiperparámetros y la versión del código, y podemos guardar los puntos de control de tu modelo para que tu proyecto sea reproducible.
2. **Organización automática**: Si le pasas un proyecto a un colaborador o te tomas unas vacaciones, W&B te facilita ver todos los modelos que hayas probado, así no pierdes horas volviendo a correr los viejos experimentos.
3. **Integración rápida y flexible**: Agrega W&B a tu proyecto en 5 minutos. Instala nuestro paquete Python, libre y de código abierto, y agrega un par de líneas a tu código, y cada vez que corras tu modelo vas a registrar las métricas y los registros.
4. **Tablero de control persistente y centralizado:** Dondequiera que entrenes tus modelos, sea en tu máquina local, tu cluster del laboratorio, o instancias de spot en la nube, te damos el mismo tablero de control centralizado. No necesitas perder el tiempo copiando y organizando archivos de TensorBoard de diferentes máquinas.
5. **Tabla poderosa**: Busca, filtra, ordena y agrupa los resultados de diferentes modelos. Es fácil revisar miles de versiones de modelos y encontrar al más adecuado para diferentes tareas. TensorBoard no funciona adecuadamente en proyectos grandes.
6. **Herramientas para la colaboración:** Utiliza W&B para organizar proyectos de aprendizaje de máquinas complejos. Es fácil compartir un enlace a W&B, y puedes utilizar equipos privados para hacer que todos envíen resultados a un proyecto compartido. También soportamos la colaboración a través de repositorios – agrega visualizaciones interactivas y describe tu trabajo con markdown. Esta es una gran forma de mantener un trabajo registrado, de compartir resultados con tu supervisor, o de presentar resultados a tu laboratorio.

 Empieza con una [cuenta personal gratuita →](http://app.wandb.ai/)

### ¿Cómo puedo configurar el nombre de la ejecución en mi código de entrenamiento?

Al principio de tu script de entrenamiento, cuando llames a wandb.init, pasa un nombre de experimento de esta forma:`wandb.init(name="my awesome run")`

### ¿Cómo obtengo el nombre de ejecución aleatorio en mi script?

Llama a `wandb.run.save()` y entonces obtén el nombre con `wandb.run.name`.

###  ¿Hay algún paquete anaconda?

No tenemos ningún paquete anaconda, pero deberías ser capaz de instalar wandb al usar:

```text
conda activate myenv
pip install wandb
```

Si tienes problemas con esta instalación, por favor háznoslo saber. Esta documentación de Anaconda acerca de [administrar paquetes](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-pkgs.html) tiene alguna guía útil.

### ¿Cómo evito que wandb escriba a la salida de mi terminal o de mi jupyter notebook?

Establece la variable de entorno [WANDB\_SILENT](https://docs.wandb.ai/library/environment-variables).

 En una notebook:

```text
%env WANDB_SILENT true
```

En un script de python:

```text
os.environ["WANDB_SILENT"] = "true"
```

###  ¿Cómo mato un trabajo con wandb?

Presiona ctrl+D en tu teclado para detener un script que esté instrumentado con wandb.

###  ¿Qué hago con mis problemas de red?

Si estás viendo errores SSL o de red:`wandb: Network error (ConnectionError), entering retry loop.` puedes intentar algunas metodologías diferentes para resolver el problema:

1. Actualiza tu certificado SSL. Si estás corriendo el script en un servidor Ubuntu, corre `update-ca-certificates`. No podemos sincronizar los registros de entrenamiento sin un certificado SSL válido, puesto que se trataría de una vulnerabilidad de seguridad.
2.  Si tu red es rara, corre el entrenamiento en [modo offline](https://docs.wandb.com/resources/technical-faq#can-i-run-wandb-offline) y sincroniza los archivos desde una máquina que tenga acceso a internet.
3.  Intenta correr [W&B Local](https://docs.wandb.ai/self-hosted/local), que opera en tu máquina y no sincroniza los archivos con nuestros servidores en la nube.

 **SSL CERTIFICATE\_VERIFY\_FAILED:** este error podría ocurrir debido al firewall de tu compañía. Puedes establecer CAs locales y entonces usar:

`export REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt`

###  ¿Qué pasa si se cae la conexión a internet mientras estoy entrenando un modelo?

Si nuestra biblioteca es incapaz de conectarse a internet, ingresará en un ciclo de reconexión y seguirá intentando enviar métricas hasta que la red sea restituida. Durante ese momento, tu programa es capaz de seguir corriendo.

Si necesitas correr en una máquina sin internet, puedes establecer WANDB\_MODE=dryrun para hacer que las métricas sean almacenas localmente, sólo en tu disco rígido. Después puedes llamar a `wandb sync DIRECTORY` para hacer que los datos sean transmitidos a nuestro servidor. 

### ¿Puedo registrar métricas en dos escalas de tiempo diferentes? \(Por ejemplo, quiero registrar la precisión del entrenamiento por lote, y la precisión de la validación por época.\)

Sí, puedes hacerlo al registrar múltiples métricas y entonces establecerles valores de un eje x. Así, en un paso podrías llamar a `wandb.log({'train_accuracy': 0.9, 'batch': 200})`, y en otro paso llamar a `wandb.log({'val_acuracy': 0.8, 'epoch': 4})`

### ¿Cómo puedo registar una métrica que no cambia a través del tiempo, tal como la precisión de la evaluación final?

Usar wandb.log\({'final\_accuracy': 0.9} funcionará bien para esto. Por defecto, wandb.log\({'final\_accuracy'}\) actualizará a wandb.settings\['final\_accuracy'\], que es el valor mostrado en la tabla de las ejecuciones.

### ¿Cómo puedo registrar diferentes métricas después de que una ejecución se completa?

Hay varias formas de hacerlo.

Para procesos de trabajo complicados, recomendamos usar múltiples ejecuciones y ajustar el parámetro del grupo en [wandb.init](https://docs.wandb.ai/library/init) a un valor único en todos los procesos que sean corridos como parte del experimento simple. La [tabla de ejecuciones](https://docs.wandb.ai/app/pages/run-page) agrupará automáticamente a la tabla por ID de grupo, y las visualizaciones se comportarán como es esperado. Esto te permitirá correr múltiples experimentos y ejecuciones de entrenamiento, mientras que los procesos separados registran todos los resultados en un lugar simple.

Para los procesos de trabajo más simples, puedes llamar a wandb.init con resume=True y id=UNIQUE\_ID, entonces después llama a wandb.init con el mismo id=UNIQUE\_ID. Entonces puedes registrar normalmente con [wandb.log](https://docs.wandb.ai/library/log) o wandb.summary, y los valores de las ejecuciones se actualizarán.

En cualquier momento, siempre puedes usar la [API](https://docs.wandb.ai/ref/export-api) para agregar métricas de evaluación adicionales.

###  ¿Cuál es la diferencia entre .log\(\) y .summary?

El summary es el valor que se muestra en la tabla, mientras que el log almacenará todos los valores para graficar más tarde.

Por ejemplo, puede que quieras llamar a `wandb.log` cada vez que accuracy cambia. Usualmente, puedes usar solamente .log. `wandb.log()`, por defecto,  también actualizará el valor de summary, a menos que hayas establecido summary manualmente para esa métrica.

El diagrama de dispersión y el diagrama de coordenadas paralelas también utilizarán el valor summary, mientras que el diagrama de líneas traza todos los valores establecidos por .log.

La razón por la que tenemos ambas opciones es que a algunas personas les gusta establecer summary manualmente, puesto que quieren que summary refleje, por ejemplo, la precisión óptima en lugar de la última precisión registrada.

### ¿Cómo instalo la biblioteca de Python wandb en entornos sin gcc?

Si intentas instalar `wandb` y ves este error:

```text
unable to execute 'gcc': No such file or directory
error: command 'gcc' failed with exit status 1
```

Puedes instalar psutil directamente desde una wheel pre construida. Encuentra la versión de tu Python y a tu sistema operativo aquí: [https://pywharf.github.io/pywharf-pkg-repo/psutil](https://pywharf.github.io/pywharf-pkg-repo/psutil)  

Por ejemplo, para instalar psutil en python 3.8 en linux:

```text
pip install https://github.com/pywharf/pywharf-pkg-repo/releases/download/psutil-5.7.0-cp38-cp38-manylinux2010_x86_64.whl/psutil-5.7.0-cp38-cp38-manylinux2010_x86_64.whl#sha256=adc36dabdff0b9a4c84821ef5ce45848f30b8a01a1d5806316e068b5fd669c6d
```

Después de que hayas instalado psutil, puedes instalar wandb con `pip install wandb`

  


