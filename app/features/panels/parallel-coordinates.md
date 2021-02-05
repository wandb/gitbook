---
description: >-
  Visualiza datos de múltiples dimensiones a través de tus experimentos de
  aprendizaje de máquinas
---

# Parallel Coordinates

Aquí hay un ejemplo de un gráfico de coordenadas paralelas. Cada eje representa algo diferente. En este caso he elegido cuatro ejes verticales. Estoy visualizando la relación entre los diferentes hiperparámetros y la precisión final de mi modelo.

* **Ejes:** Diferentes hiperparámetros de [wandb.config](https://docs.wandb.ai/library/config) y métricas de [wandb.log\(\)](https://docs.wandb.ai/library/log).
* **Líneas:** Cada línea representa una ejecución simple. Pasa el cursor del mouse sobre una línea para ver una descripción emergente con los detalles de la ejecución. Van a ser mostradas todas las líneas que se correspondan con los filtros actuales, pero si desactivas al ojo, las líneas se van a deshabilitar.

 **Ajustes del Panel**

Configura estas características en los ajustes del panel – haz click en el botón de edición, en la esquina superior derecha del panel.

* Descripción emergente: Al pasar el cursor del mouse, se muestra una leyenda con la información de cada ejecución.. 
* Títulos: Edita los títulos de los ejes para que sean más legibles.. 
* Gradiente: Personaliza al gradiente para que esté en los rangos de colores que desees.. 
* Escala del registro: Cada eje puede ser establecido para verse en una escala del registro de forma independiente. 
* Da vuelta el eje: Cambia la dirección del eje – esto es útil cuando tengas como columnas tanto a la precisión como a la pérdida

 [Míralo en tiempo real →](https://app.wandb.ai/example-team/sweep-demo/reports/Zoom-in-on-Parallel-Coordinates-Charts--Vmlldzo5MTQ4Nw)

![](../../../.gitbook/assets/2020-04-27-16.11.43.gif)

