---
description: >-
  Cada ejecución de entrenamiento de tu modelo obtiene una página dedicada, que
  está organizada dentro de un proyecto mayor.
---

# Run Page

Usa la página de la ejecución para explorar la información detallada acerca de una versión simple de tu modelo.

## Pestaña Resumen

* Nombre, descripción y etiquetas de la ejecución
* Nombre del host, sistema operativo, versión de Python, y comando que lanzó la ejecución
* Lista de los parámetros de configuración guardados con [wand](https://docs.wandb.ai/library/config)[b](https://docs.wandb.ai/library/config)[.config](https://docs.wandb.ai/library/config).
* Lista de los parámetros de la síntesis guardados con [wandb.log\(\)](https://docs.wandb.ai/library/log), establecido por defecto al último valor registrado

 [Ver un ejemplo en tiempo real →](https://app.wandb.ai/carey/pytorch-cnn-fashion/runs/munu5vvg/overview?workspace=user-carey)

![Pesta&#xF1;a del resumen de la ejecuci&#xF3;n en el Tablero de Control de W&amp;B](../../.gitbook/assets/wandb-run-overview-page.png)

Los detalles de Python son privados, incluso si haces que la página sea de uso público. Aquí hay un ejemplo de mi página de ejecuciones en modo incógnito a la izquierda, y mi cuenta a la derecha.

![](../../.gitbook/assets/screen-shot-2020-04-07-at-7.46.39-am.png)

## Pestaña Gráficos

* Busca, agrupa y arregla las visualizaciones
* Haz click sobre el ícono✏️  del lápiz sobre un gráfico para editarlo
  * Cambia el eje x, las métricas y los rangos
  * edita las leyendas, los títulos y los colores de los gráficos
* Mira predicciones de los ejemplos desde tu conjunto de validación
* Para obtener estos gráficos, registra los datos con [wandb.log\(\)](https://docs.wandb.ai/library/log)

[Ver un ejemplo en tiempo real →](https://app.wandb.ai/wandb/examples-keras-cnn-fashion/runs/wec25l0q?workspace=user-carey)

![](../../.gitbook/assets/wandb-run-page-workspace-tab%20%281%29.png)

## Pestaña Sistema

* Visualiza la utilización de la CPU, la memoria del sistema, la entrada/salida del disco, el tráfico de red, la utilización de la GPU, la temperatura de la GPU, el tiempo de la GPU gastado en acceder a la memoria, la memoria asignada a la GPU, y el uso de la potencia de la GPU.
*  Lambda Labs destacó cómo utilizar las métricas del sistema de W&B en un [artículo del blog →](https://lambdalabs.com/blog/weights-and-bias-gpu-cpu-utilization/)

 [Ver un ejemplo en tiempo real →](https://wandb.ai/stacey/deep-drive/runs/ki2biuqy/system?workspace=user-carey)

![](../../.gitbook/assets/wandb-system-utilization.png)

### Pestaña Modelo

* Mira las capas de tu modelo, el número de parámetros, y la forma de la salida de cada capa.

[Ver un ejemplo en tiempo real →](https://app.wandb.ai/stacey/deep-drive/runs/pr0os44x/model)

![](../../.gitbook/assets/wandb-run-page-model-tab.png)

## Pestaña Registros

* Salida impresa en la línea de comandos, el stdout y el stderr de la máquina entrenando al modelo
* Mostramos las últimas 1000 líneas. Después de que la ejecución ha finalizado, si quisieras descargar el archivo del registro completo, haz click en el botón descarga, en la esquina superior derecha.

[Ver un ejemplo en tiempo real →](https://app.wandb.ai/stacey/deep-drive/runs/pr0os44x/logs)

![](../../.gitbook/assets/wandb-run-page-log-tab.png)

## Pestaña Archivos

* Guarda los archivos para sincronizarlos con la ejecución usando [wandb.save\(\)](https://docs.wandb.ai/library/save)
* Mantén los puntos de control del modelo, los ejemplos del conjunto de validación, y más
* Utiliza el diff.patch para [restituir](https://docs.wandb.ai/library/restore) la versión exacta de tu código

🌟 Nueva recomendación: Prueba los [Art](https://docs.wandb.ai/artifacts)[e](https://docs.wandb.ai/artifacts)[fact](https://docs.wandb.ai/artifacts)[o](https://docs.wandb.ai/artifacts)[s](https://docs.wandb.ai/artifacts) para hacer el seguimiento de las entradas y de las salidas.

[Ver un ejemplo en tiempo real →](https://app.wandb.ai/stacey/deep-drive/runs/pr0os44x/files/media/images)

![](../../.gitbook/assets/wandb-run-page-files-tab.png)

