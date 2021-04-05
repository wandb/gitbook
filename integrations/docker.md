# Docker

###  Integración de Docker

W&B puede almacenar un puntero a la imagen de Docker en la que se ejecutó tu código, dándote la capacidad de restituir un experimento previo exactamente al mismo entorno en el que fue corrido. La biblioteca wandb busca la variable de entorno **WANDB\_DOCKER** para persistir este estado. Proveemos algunos asistentes que establecen automáticamente dicho estado.

###  Desarrollo Local

 `wandb docker` es un comando que arranca un contenedor docker, le pasa las variables de entorno de wandb, monta tu código, y se asegura de que wandb sea instalado. Por defecto, el comando utiliza una imagen de docker con TensorFlow, PyTorch, Keras y Jupyter instalados. Puedes usar el mismo comando para empezar tu propia imagen de docker: `wandb docker my/image:latest`. El comando monta el directorio actual en el directorio “/app” del contenedor, puedes cambiarlo con la bandera “--dir”.

### Producción

El comando `wandb docker-run` es provisto para las cargas de trabajo en producción. La intención es que sea un reemplazo de `nvidia-docker`. Es un wrapper simple para el comando docker run que agrega tus credenciales y la variable de entorno **WANDB\_DOCKER** a la llamada. Si no pasas la bandera “--runtime” y nvidia-docker está disponible en tu máquina, también se asegura de que el runtime sea establecido a nvidia.

### Kubernetes

Si ejecutas las cargas de trabajo de entrenamiento en Kubernetes y la API de k8s es expuesta al pod \(que es el caso por defecto\), wandb le pedirá a la API el digest de la imagen de docker y va a establecer automáticamente la variable de entorno **WANDB\_DOCKER.**

##  Restitución

 Si una ejecución fue instrumentada con la variable de entorno **WANDB\_DOCKER**, al llamar a `wandb restore username/project:run_id` hará el checkout a una nueva rama que restituye tu código, entonces lanzará la misma imagen de docker usada para el entrenamiento, precargada con el comando original.

