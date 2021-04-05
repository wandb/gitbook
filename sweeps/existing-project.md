---
description: >-
  Si ya estás usando en tu proyecto a wandb.init, wandb.config y wandb.log,
  ¡comienza aquí!
---

# Sweep from an existing project

 Si tienes un proyecto existente en W&B, es fácil empezar a optimizar tus modelos con los barridos de los hiperparámetros. Voy a guiarte respecto a los pasos que hay que seguir, a través de un ejemplo que está funcionando – puedes abrir mi [panel de control de W&B](https://app.wandb.ai/carey/pytorch-cnn-fashion). Estoy usando el código de [este repositorio de ejemplo](https://github.com/wandb/examples/tree/master/examples/pytorch/pytorch-cnn-fashion), que entrena una red neuronal circunvolucional de PyTorch para clasificar imágenes desde el [conjunto de datos Fashion MNIST](https://github.com/zalandoresearch/fashion-mnist).

## 1. Crea un proyecto

Corre la primera ejecución de referencia manualmente para verificar que el registro en W&B esté funcionando apropiadamente. Vas a descargar este modelo de ejemplo simple, lo vas a entrenar por algunos minutos, y vas a ver que el ejemplo aparece en el tablero de control web.

* Clona este repositorio git clone `git clone https://github.com/wandb/examples.git`
* Abre este ejemplo  `cd examples/pytorch/pytorch-cnn-fashion`
* Corre una ejecución manualmente`python train.py`

 [Mira la página del proyecto de ejemplo →](https://app.wandb.ai/carey/pytorch-cnn-fashion)

## 2. Crea un barrido

Desde tu página del proyecto, abre la pestaña Barrido, en la barra lateral, y haz click en “Crear Barrido”.

![](../.gitbook/assets/sweep1.png)

 Los barridos son una herramienta para encontrar los valores óptimos de los hiperparámetros. Queremos hacer que la búsqueda de los hiperparámetros sea transparente, poderosa y fácil de usar.Para aprender más acerca de los barridos, verifica nuestra documentación.Para comenzar un barrido haz click en el botón “Crear Barrido”.

![](../.gitbook/assets/sweep2.png)

## 3. Configuración del Barrido

 Edita la configuración del barrido antes de lanzar el mismo. A partir de las ejecuciones existentes, nosotros suponemos buenos valores por defecto para los rangos del barrido, los cuales son ligeramente más amplios que los rangos existentes de los valores que has registrado.

![](../.gitbook/assets/sweep3.png)

That’s it! Now you're running a sweep. Here’s what the dashboard looks like as my example sweep gets started. [View an example project page →](https://app.wandb.ai/carey/pytorch-cnn-fashion)

![](https://paper-attachments.dropbox.com/s_5D8914551A6C0AABCD5718091305DD3B64FFBA192205DD7B3C90EC93F4002090_1579066494222_image.png)

## Planta un nuevo barrido con ejecuciones existentes

Lanza un nuevo barrido utilizando ejecuciones existentes que hayas registrado previamente.

1. Abre tu tabla del proyecto.
2. Selecciona las ejecuciones que quieras usar con las casillas de selección en el lado izquierdo de la tabla.
3. Haz click en el menú desplegable para crear un nuevo barrido.

Ahora tu barrido se va a establecer en nuestro servidor. Todo lo que necesitas hacer es lanzar uno o más agentes para comenzar a correr las ejecuciones.

![](../.gitbook/assets/create-sweep-from-table%20%281%29%20%281%29.png)

