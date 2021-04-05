---
description: >-
  Ejecuta la búsqueda y los algoritmos de detención localmente, en lugar de usar
  nuestro servicio alojado en la nube
---

# Local Controller

Por defecto, el controlador de los hiperparámetros es alojado por W&B como un servicio en la nube. Los agentes de W&B se comunican con el controlador para determinar el próximo conjunto de parámetros que hay que usar para el entrenamiento. El controlador también es responsable de ejecutar los algoritmos de detención temprana, para determinar qué ejecuciones pueden ser detenidas.

La característica del controlador local permite que el usuario ejecute los algoritmos de búsqueda y de detención de forma local. El controlador local le da al usuario la capacidad de inspeccionar e instrumentar al código para depurar los problemas, así también como para desarrollar nuevas características que pueden ser incorporadas en el servicio de la nube.

{% hint style="info" %}
En la actualidad, el controlador local está limitado a correr un agente simple.
{% endhint %}

## Configuración del controlador local

Para habilitar al controlador local, agrega lo siguiente al archivo de configuración del barrido:

```text
controller:
  type: local
```

## Ejecutando el controlador local

 El siguiente comando va a lanzar un controlador del barrido:

```text
wandb controller SWEEP_ID
```

  
Alternativamente, puedes lanzar un controlador cuando inicializas el barrido:

```text
wandb sweep --controller sweep.yaml
```

