---
description: Búsqueda de hiperparámetros y optimización del modelo
---

# Sweeps

Utiliza los Barridos de Weights & Biases para automatizar la optimización de los hiperparámetros y para explorar el espacio de los posibles modelos.

##  Beneficios de usar los Barridos de W&B

1. Ajuste veloz: con sólo algunas líneas de código puedes ejecutar barridos de W&B.
2.  Transparente: Citamos todos los algoritmos que estamos usando, y [nuestro código es abierto](https://github.com/wandb/client/tree/master/wandb/sweeps).
3. Poderoso: Nuestros barridos son completamente personalizables y configurables. Puedes lanzar un barrido entre docenas de máquinas, y es tan sencillo como arrancar un barrido desde tu laptop.

## Casos de Uso Comunes

1.  Explora: Muestrea eficientemente el espacio de las combinaciones de los hiperparámetros para descubrir regiones prometedoras y para obtener una intuición acerca de tu modelo.
2. Optimiza: Utiliza barridos para encontrar un conjunto de hiperparámetros con un desempeño óptimo.
3. Validación cruzada de k iteraciones: Aquí hay [un breve código de ejemplo](https://github.com/wandb/examples/tree/master/examples/wandb-sweeps/sweeps-cross-validation) de la validación cruzada de k iteraciones con barridos de W&B.

## Estrategia

1. Agrega wandb: En tu script de Python, agrega un par de líneas de código para registrar los hiperparámetros y para producir las métricas desde tu script. [Empieza ahora →](https://docs.wandb.ai/sweeps/quickstart)
2. Escribe la configuración: Define las variables y los rangos sobre los cuales hacer el barrido. Selecciona una estrategia de búsqueda – soportamos búsquedas en rejilla, aleatorias y optimizaciones Bayesianas, así también como la detención temprana. Revisa [aquí](https://github.com/wandb/examples/tree/master/examples/keras/keras-cnn-fashion) algunas configuraciones de ejemplo.
3.  Inicializa el barrido: Lanza el servidor del barrido. Nosotros alojamos este controlador central y coordinamos entre los agentes que ejecutan el barrido.
4. Lanza el\(los\) agente\(s\): Ejecuta este comando en cada máquina que te gustaría usar para entrenar a los modelos en el barrido. Los agentes le pedirán al servidor central de barridos qué hiperparámetros probar a continuación, y entonces van a correr las ejecuciones.
5. Visualiza los resultados: Abre nuestro tablero de control para ver todos los resultados en un lugar centralizado.

![](../.gitbook/assets/central-sweep-server-3%20%282%29%20%282%29.png)

{% page-ref page="quickstart.md" %}

{% page-ref page="existing-project.md" %}

{% page-ref page="configuration.md" %}

{% page-ref page="local-controller.md" %}

{% page-ref page="python-api.md" %}

{% page-ref page="sweeps-examples.md" %}

