---
description: Recursos para que la gente empiece con el Aprendizaje de Máquinas
---

# Beginner FAQ

## ¿Estás aprendiendo aprendizaje de máquinas?

Wandb es una herramienta para visualizar el entrenamiento y esperamos que sea útil para todo el mundo, desde expertos hasta personas que recién están empezando. Si tienes preguntas generales acerca del aprendizaje de máquinas, eres bienvenido a hacerlas en nuestro canal de [Slack](http://wandb.me/slack). También hemos realizado algunos [video tutoriales](https://www.wandb.com/tutorials) gratuitos, con código de ejemplo, diseñados para que empieces.

 Una gran forma de aprender aprendizaje de máquinas es empezar con un proyecto interesante. Si no tienes ningún proyecto en mente, un buen lugar para encontrar proyectos es nuestra página de [referencias](https://www.wandb.com/benchmarks) – tenemos una variedad de tareas de aprendizaje de máquinas, con datos y código funcionando, que puedes mejorar.

## Recursos En Línea

Hay muchos recursos en línea excelentes para aprender acerca del aprendizaje de máquinas. Por favor, envíanos una nota si deberíamos agregar algo aquí.

* [fast.ai ](https://www.fast.ai)- Excelentes clases prácticas de aprendizaje de máquinas y una comunidad amigable.
* [deep learning book](http://www.deeplearningbook.org) - Libro detallado disponible en línea y de forma gratuita.
* [Stanford CS229](https://see.stanford.edu/Course/CS229) - Conferencias de una gran clase disponible en línea.

## Buscando el sesgo en los modelos

 Si estás entrenando un modelo de aprendizaje de máquinas, querrás ser capaz de visualizar cómo se ejecuta con diferentes entradas. Un problema común, especialmente cuando estás empezando, es que es difícil establecer aquellas visualizaciones. Aquí es donde Weights & Biases entra en juego. Hacemos que sea fácil obtener las métricas para entender el desempeño de tu modelo.

Aquí hay un ejemplo hipotético – estás entrenando un modelo para identificar objetos en una autopista. Tu conjunto de datos es un puñado de imágenes etiquetadas con coches, peatones, bicicletas, árboles, construcciones, etc. Mientras entrenas tu modelo, puedes visualizar las diferentes precisiones de las clases. Esto significa que puedes ver si tu modelo es magnífico encontrando vehículos, pero malo hallando peatones. Este podría ser un sesgo peligroso, especialmente en un modelo de conducción automática de vehículos.

 ¿Estás interesado en ver un ejemplo en tiempo real? Aquí hay un reporte que compara la precisión del modelo identificando imágenes de diferentes tipos de plantas y animales – pájaros, mamíferos, hongos, etc. Los gráficos de Weights & Biases facilitan ver cómo se desempeña cada versión del modelo \(cada línea en el gráfico\) sobre diferentes clases.

 [Ver el reporte en W&B →](https://app.wandb.ai/stacey/curr_learn/reports/Species-Identification--VmlldzoxMDk3Nw)

![](../.gitbook/assets/image%20%2818%29%20%283%29%20%283%29.png)

