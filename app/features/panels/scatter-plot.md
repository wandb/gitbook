# Scatter Plot

 Utiliza el diagrama de dispersión para comparar múltiples ejecuciones y para visualizar cómo se están desempeñando tus experimentos. Hemos agregado algunas características ajustables:

1. Diagrama una línea junto con el mínimo, el máximo y el promedio
2. Personaliza las descripciones emergentes de los metadatos
3. Controla los colores de los puntos
4. Establece los rangos de los ejes5. Intercambia los ejes para registrar la escala

Aquí hay un ejemplo de la precisión de la validación de diferentes modelos a lo largo de un par de semanas de experimentación. La descripción emergente está personalizada para incluir el tamaño del lote y la dilución, así también como los valores sobre los ejes. También hay una línea diagramando el promedio móvil de la precisión de la validación.  
[Ver un ejemplo en tiempo real →](https://app.wandb.ai/l2k2/l2k/reports?view=carey%2FScatter%20Plot)

![](https://paper-attachments.dropbox.com/s_9D642C56E99751C2C061E55EAAB63359266180D2F6A31D97691B25896D2271FC_1579031258748_image.png)

## Preguntas Comunes

###  ¿Es posible diagramar el máximo de una métrica, en lugar de hacerlo paso a paso?

La mejor forma de hacer esto es crear un Diagrama de Dispersión de la métrica, ir al menú Edición, y seleccionar Anotaciones. Desde ahí puedes graficar el máximo móvil de los valores.

