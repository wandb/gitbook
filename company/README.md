# Company

### ¿Qué es wandb?

Wandb es una herramienta de seguimiento de experimentos para el aprendizaje de máquinas. Le damos facilidades a todos los que estén haciendo aprendizaje de máquinas, para que hagan seguimiento de los experimentos y compartan los resultados con los colegas y con sus futuros ellos mismos.

Aquí hay un video de reseña de 1 minuto. [Mira un proyecto de ejemplo →](https://app.wandb.ai/stacey/estuary)

{% embed url="https://www.youtube.com/watch?v=icy3XkZ5jBk" %}

###  ¿Cómo funciona?

Cuando instrumentas tu código de entrenamiento con wandb, nuestro proceso en segundo plano va a recoger datos útiles respecto a lo que está ocurriendo mientras entrenas tus modelos. Por ejemplo, podemos hacer el seguimiento de las métricas de desempeño del modelo, de los hiperparámetros, de los gradientes, de las métricas del sistema, de los archivos de salida, y de tu git commit más reciente.

{% page-ref page="../ref/export-api/examples.md" %}

### ¿Qué difícil es establecerlo?

Sabemos que la mayoría de la gente hace el seguimiento de sus entrenamientos con herramientas como emacs o Google Sheets, así que hemos diseñado a wandb para que sea tan liviana como sea posible. La integración debería tomar entre 5 y 10 minutos, y wandb no va a ralentizar o a romper a tu script de entrenamiento.

## Beneficios de wandb

Nuestros usuarios nos dicen que obtienen tres clases de beneficios al usar wandb:

### 1.  Visualiza el entrenamiento

Algunos de nuestros usuarios ven a wandb como a un “TensorBoard persistente”. Por defecto, recogemos las métricas de desempeño del modelo, como la precisión y la pérdida. También recogemos y visualizamos objetos matplotlib, archivos del modelo, métricas del sistema como el uso de GPU, y el SHA de tu git commit más reciente más un archivo patch con los cambios desde el último commit.También puedes tomar notas acerca de las ejecuciones individuales, para que sean guardadas con tus datos de entrenamiento. 

Aquí hay un [proyecto de ejemplo](https://app.wandb.ai/bloomberg-class/imdb-classifier/runs/2tc2fm99/overview) relevante a partir de una clase que impartimos en Bloomberg.

### 2. Organiza y compara muchas ejecuciones de entrenamiento

La mayoría de las personas que entrenan modelos de aprendizaje de máquinas están probando muchísimas versiones de su modelo, y nuestro objetivo es ayudar a que estas personas estén organizadas.

Puedes crear proyectos para hacer el seguimiento de todas tu ejecuciones en un lugar simple. 

Puedes visualizar las métricas del desempeño a través de muchas ejecuciones y filtros, agruparlas y etiquetarlas de cualquier forma que lo desees.Un buen proyecto de ejemplo es el [proyecto estuary](https://app.wandb.ai/stacey/estuary) de Stacey.En la barra lateral puedes activar o desactivar las ejecuciones que se van a mostrar en los gráficos, o hacer click en una de ellas para obtener más información. Todas tus ejecuciones son guardadas y organizadas para ti en un entorno de trabajo unificado.



![](../.gitbook/assets/image%20%2885%29%20%281%29%20%282%29%20%283%29%20%283%29%20%283%29%20%283%29%20%284%29%20%283%29%20%281%29%20%283%29.png)

### 3. Comparte tus resultados

Una vez que hayas hecho muchas ejecuciones, por lo general querrás organizarlas para mostrar alguna clase de resultado. Nuestros amigos en Latent Space escribieron un artículo genial llamado [Las Mejores Prácticas de ML: Desarrollo Dirigido Por Tests](https://www.wandb.com/articles/ml-best-practices-test-driven-development), que habla acerca de cómo utilizan los reportes de W&B para mejorar la productividad de su equipo.

Un usuario, Boris Dayma, escribió un reporte de ejemplo público sobre la [Segmentación Semántica](https://app.wandb.ai/borisd13/semantic-segmentation/reports?view=borisd13%2FSemantic%20Segmentation%20Report). Él hace un recorrido a través de varias metodologías que intentó y comenta cuán bien le funcionaron.

Realmente esperamos que wandb aliente a los equipos de ML a colaborar más productivamente.Si deseas aprender más acerca de cómo los equipos utilizan wandb, hemos registrados algunas entrevistas con nuestros usuarios técnicos en [OpenAI](https://www.wandb.com/articles/why-experiment-tracking-is-crucial-to-openai) y [Toyota Research](https://www.youtube.com/watch?v=CaQCw-DKiO8).

## Equipos

Si estás trabajando en un proyecto de aprendizaje de máquinas con colaboradores, compartir resultados es muy fácil.

* [Equipos Empresariales](https://www.wandb.com/pricing): Apoyamos a equipos de pequeñas startups y de grandes empresas como OpenAI y Toyota Research Institute. Tenemos opciones de tarifas flexibles que se adecuan a las necesidades de tu equipo, y soportamos servidores en la nube, nubes privadas, e instalaciones en entornos locales.
* [Equipos Académicos](https://www.wandb.com/academic): Estamos dedicados a apoyar a la investigación académica transparente y colaborativa. Si eres un académico, te daremos acceso a equipos gratuitos para compartir tu investigación en los proyectos privados.

Si te gustaría compartir un proyecto con gente que esté fuera de un equipo, has click en los ajustes de privacidad del proyecto, en la barra de navegación, y establece el proyecto a “Público”. Cualquiera con el que compartas el enlace va a ser capaz de ver los resultados de tu proyecto público.

