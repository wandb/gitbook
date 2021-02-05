---
description: >-
  Visualiza las relaciones entre los hiperparámetros de tu modelo y las métricas
  de salida
---

# Parameter Importance

 Este panel emerge de cuáles de tus hiperparámetros fueron los mejores indicadores, y está altamente correlacionado con los valores deseables de tus métricas.

![](https://paper-attachments.dropbox.com/s_B78AACEDFC4B6CE0BF245AA5C54750B01173E5A39173E03BE6F3ACF776A01267_1578795733856_image.png)

**Correlación** es una correlación lineal entre el hiperparámetro y la métrica elegida \(en este caso val\_loss\). Así que una correlación alta significa que cuando el hiperparámetro tiene un valor más alto, la métrica también tiene valores más altos, y viceversa. La correlación es una métrica magnífica, a la cual hay que prestarle atención, pero no puede capturar interacciones de segundo orden entre las entradas, y puede volverse complicado comparar entradas con rangos extremadamente diferentes.

Correlación es una correlación lineal entre el hiperparámetro y la métrica elegida \(en este caso val\_loss\). Así que una correlación alta significa que cuando el hiperparámetro tiene un valor más alto, la métrica también tiene valores más altos, y viceversa. La correlación es una métrica magnífica, a la cual hay que prestarle atención, pero no puede capturar interacciones de segundo orden entre las entradas, y puede volverse complicado comparar entradas con rangos extremadamente diferentes.Consecuentemente, también calculamos una métrica de importancia, en donde entrenamos un bosque aleatorio con los hiperparámetros como entradas y la métrica como la salida objetivo, y reportamos los valores de importancia de la característica para el bosque aleatorio.La idea para esta técnica estuvo inspirada en una conversación con [Jeremy Howard](https://twitter.com/jeremyphoward), quien fue pionero en utilizar las importancias de las características del bosque aleatorio para explorar los espacios de los hiperparámetros en [Fast.ai](http://fast.ai/). Te recomendamos fuertemente que revises esta fenomenal [conferencia](http://course18.fast.ai/lessonsml1/lesson4.html) \(y estas [notas](https://forums.fast.ai/t/wiki-lesson-thread-lesson-4/7540)\) para aprender más acerca de la motivación detrás de este análisis.

Este panel acerca de la importancia de los hiperparámetros desenmaraña las complicadas interacciones entre los hiperparámetros altamente correlacionados. Al hacerlo, te ayuda a ajustar las búsquedas de tus hiperparámetros al demostrarte cuáles son los que más importan en términos de predecir el desempeño del modelo.

## Creando un Panel de Importancia de los Hiperparámetros

Ve a tu Proyecto de Weights & Biases. Si no tienes uno, puedes usar [este proyecto](https://app.wandb.ai/sweep/simpsons).

Desde la página de tu proyecto, haz click en **Agregar Visualización.**

![](https://paper-attachments.dropbox.com/s_B78AACEDFC4B6CE0BF245AA5C54750B01173E5A39173E03BE6F3ACF776A01267_1578795570241_image.png)

Entonces, elige **Importancia de los Parámetros.**

No necesitas escribir ningún código nuevo, además del necesario para [integrar a Weights & Biases](https://docs.wandb.com/quickstart) en tu proyecto.

![](https://paper-attachments.dropbox.com/s_B78AACEDFC4B6CE0BF245AA5C54750B01173E5A39173E03BE6F3ACF776A01267_1578795636072_image.png)

## Interpretando A Un Panel de Importancia De Los Hiperparámetros

![](https://paper-attachments.dropbox.com/s_B78AACEDFC4B6CE0BF245AA5C54750B01173E5A39173E03BE6F3ACF776A01267_1578798509642_image.png)

Este panel te muestra todos los parámetros pasados al objeto [wandb.config](https://docs.wandb.com/library/python/config) en tu script de entrenamiento. A continuación, muestra las importancias y las correlaciones de la característica de estos parámetros de configuración, con respecto a la métrica del modelo que selecciones \(`val_loss` en este caso\). 

### Importancia

La columna importancia te muestra el grado en el que cada hiperparámetro fue útil para predecir la métrica elegida. Podemos imaginar un escenario en el que comenzamos a ajustar una plétora de hiperparámetros y usamos este gráfico para ir puliendo cuáles son los que merecen más exploración. Los barridos subsecuentes entonces pueden estar limitados a los hiperparámetros más importantes, lo que permite encontrar, consecuentemente, un mejor modelo, más rápido y más barato.

Nota: Calculamos estas importancias usando un modelo basado en un árbol, en vez de un modelo lineal, puesto que los primeros son más tolerantes, tanto con los datos categóricos como con los datos que no están normalizados.En el panel antes mencionado, podemos ver que `epochs`, `learning_rate`, `batch_size` y `weight_decay` fueron bastante importantes.

 Como próximo paso, podríamos ejecutar otro barrido explorando valores más finos de estos hiperparámetros. Interesantemente, mientras que `learning_rate` y `batch_size` fueron importantes, estos no se correlacionaron bien con la salida.  
Esto nos lleva a las correlaciones.

### Correlaciones

Las correlaciones capturan relaciones lineales entre los hiperparámetros individuales y los valores de las métricas. Ellas responden a la pregunta - ¿existe una relación significativa entre usar un hiperparámetro, digamos el optimizador SGD, y mi val\_loss? \(la respuesta en este caso es sí\). Los valores de la correlación van de -1 a 1, en donde los valores positivos representan una correlación lineal positiva, los valores negativos representan una correlación lineal negativa, y un valor de 0 no representa ninguna correlación. Por lo general, un valor mayor a 0.7, en cualquier dirección, representa una correlación fuerte.

Podríamos usar este gráfico para explorar aún más los valores que tengan una correlación más alta a la de nuestra métrica \(en este caso, podríamos tomar el descenso del gradiente estocaśtico, o adam sobre rmsprop o nadam\) o entrenar por más épocas.

 Nota rápida sobre la interpretación de las correlaciones:

* las correlaciones muestran evidencia de asociación, no necesariamente causalidad.
*  las correlaciones son sensibles a los valores atípicos, lo que podría convertir a una relación fuerte en una moderada, específicamente si el tamaño de la muestra de los hiperparámetros probados es pequeña.
* y finalmente, las correlaciones solamente capturan las relaciones lineales entre los hiperparámetros y las métricas. 
* Si existe una relación polinomial fuerte, ésta no va a ser capturada por las correlaciones.

Las disparidades entre la importancia y las correlaciones son consecuencia del hecho de que la importancia explica las interacciones entre los hiperparámetros, mientras que la correlación sólo mide las influencias de los hiperparámetros individuales sobre los valores de las métricas. 

Segundo, las correlaciones sólo capturan las relaciones lineales, mientras que las importancias pueden capturar relaciones más complejas.

Como puedes observar, tanto la importancia como las correlaciones son herramientas poderosas para entender cómo influyen los hiperparámetros en el desempeño del modelo.Esperamos que este panel te ayude a capturar estos conocimientos y que los perfecciones más rápidamente en un modelo poderoso.

