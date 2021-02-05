---
description: Métricas que son registradas automáticamente por wandb
---

# System Metrics

`wandb` registra de forma automática a las métricas del sistema cada 2 segundos, promediadas sobre un período de 30 segundos. Las métricas incluyen:

* **Utilización de la CPU.** 
* **Utilización de la Memoria del Sistema.** 
* **Utilización de la Entrada/Salida del Disco.** 
* **Tráfico de Red \(bytes enviados y recibidos\).** 
* **Utilización de la GPU.** 
* **Temperatura de la GPU.** 
* **Tiempo de la GPU Gastado en Acceder a la Memoria \(como un porcentaje del tiempo de muestreo\).**
* **Memoria Asignada de la GPU**

Las métricas de la GPU son recopiladas por cada dispositivo usando [nvidia-ml-py3](https://github.com/nicolargo/nvidia-ml-py3/blob/master/pynvml.py). Para más información de cómo interpretar estas métricas y de cómo optimizar el desempeño de tu modelo, mira [esta publicación útil desde el blog de Lambda Labs](https://lambdalabs.com/blog/weights-and-bias-gpu-cpu-utilization/).

