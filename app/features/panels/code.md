# Code Saving

Por defecto, sólo guardamos el último hash del git commit. Puedes activar más características de código para comparar el código entre tus experimentos, de forma dinámica, en la interfaz de usuario.

Comenzando con la versión 0.8.28 de `wandb`, podemos guardar el código de tu archivo de entrenamiento principal, desde donde llamas a `wandb.init()`. 

Esto se va a sincronizar con el tablero de control y se va a mostrar en una pestaña en la página de las ejecuciones, así también como en el panel Comparador de Código. Ve a la [página de ajustes](https://app.wandb.ai/settings) para habilitar el guardado de código por defecto.

![Here&apos;s what your account settings look like. You can save code by default.](../../../.gitbook/assets/screen-shot-2020-05-12-at-12.28.40-pm.png)

## Valores Predeterminados del Proyecto

Cuando creas un nuevo proyecto, estos son los ajustes por defecto que vamos a utilizar

![](../../../.gitbook/assets/cc1.png)

##  Historial de las Sesiones en Jupyter

Comenzando con la versión 0.8.34 de **wandb**, nuestra biblioteca guarda las sesiones de Jupyter. Cuando llamas a **wandb.init\(\)** desde dentro de Jupyter, agregamos un enganche para guardar automáticamente una notebook de Jupyter que contiene el historial del código ejecutado en tu sesión actual. Puedes encontrar este historial de sesiones en un navegador de archivos de ejecuciones, debajo del directorio del código:

![](../../../.gitbook/assets/cc2%20%284%29%20%284%29.png)

Al hacer click en este archivo se van a mostrar las celdas que fueron ejecutadas en tu sesión, conjuntamente con cualquier salida creada al llamar al método display de iPython. Esto te permite ver exactamente qué código fue ejecutado dentro de Jupyter en una ejecución dada. Cuando sea posible, también guardamos la versión más reciente de la notebook, que también encontrarías en el directorio del código.

![](../../../.gitbook/assets/cc3%20%283%29%20%281%29.png)

## Haciendo diff en Jupyter

Una última característica extra es la capacidad de hacer diff en las notebooks. En lugar de mostrar al JSON sin procesar en nuestro panel del Comparador del Código, extraemos cada celda y visualizamos cualquier línea que haya cambiado. Hemos planificado algunas características interesantes para integrar a Jupyter aún más a nuestra plataforma.

