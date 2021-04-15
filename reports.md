---
description: >-
  Administración de proyectos y herramientas de colaboración para proyectos de
  aprendizaje de máquinas
---

# Reports

Los reportes te permiten organizar visualizaciones, describir tus conclusiones, y compartir actualizaciones con los colaboradores.

### Casos de Uso

1. **Notas**: Agrega un gráfico con una nota breve.
2. **Colaboración**: Comparte conclusiones con tus colegas.
3. **Registro de trabajo**: Haz el seguimiento de lo que has intentado, y planifica los próximos pasos.

### [Mira el caso de estudio de OpenAI →](https://bit.ly/wandb-learning-dexterity)

Una vez que tengas los [experimentos en W&B](https://docs.wandb.ai/quickstart), visualiza fácilmente los resultados en los reportes. Aquí puedes ver un video de una reseña rápida.

{% embed url="https://www.youtube.com/watch?v=o2dOSIDDr1w" caption="" %}

## Colabora sobre los reportes

Una vez que hayas guardado un reporte, puedes hacer click en el botón Compartir para colaborar. Asegúrate de que los ajustes de visibilidad en tu proyecto permitan que tus colaboradores accedan al reporte – vas a necesitar un proyecto abierto o un proyecto en equipo para compartir un reporte que puedas editar en conjunto.

Cuando presionas editar, estarás editando una copia borrador del reporte. Este borrador se guarda automáticamente, y cuando presiones **Guardar a reporte** vas a publicar los cambios al reporte compartido.

Si mientras tanto uno de tus colaboradores ha editado el reporte, obtendrás una advertencia que te va a ayudar a resolver potenciales conflictos de edición.

![](.gitbook/assets/collaborative-reports.gif)



### Comenta sobre los reportes

Haz click en el botón comentario, sobre un panel en un reporte, para agregar un comentario directamente a dicho panel.

![](.gitbook/assets/demo-comment-on-panels-in-reports.gif)

## Grillas de Paneles

Si te gustaría comparar un conjunto diferente de ejecuciones, crea una nueva grilla de paneles. Cada gráfico de la sección es controlado por los Conjuntos de Ejecuciones en la parte inferior de dicha sección.

## Conjuntos de Ejecuciones

* **Conjuntos de ejecuciones dinámicos**: Si comienzas desde “Visualizar todo” y filtras o deseleccionas las ejecuciones a visualizar, entonces el conjunto de ejecuciones se va a actualizar automáticamente para mostrar cualquier nueva ejecución que coincida con los filtros.
* **Conjuntos de ejecuciones estáticos:** Si comienzas desde “No visualizar a ninguna” y seleccionas a las ejecuciones que quieras incluir en tu conjunto de ejecuciones, sólo vas a obtener a aquellas ejecuciones en el conjunto de ejecuciones. No va a ser agregada ninguna ejecución nueva.
* **Definiendo claves:** Si tienes múltiples Conjuntos de Ejecuciones en una sección, las columnas quedan definidas por el primer conjunto de ejecuciones. Para mostrar diferentes claves de diferentes proyectos, puedes hacer click en “Agregar Grilla de Paneles” para agregar una nueva sección de gráficos y conjuntos de ejecuciones con ese segundo conjunto de claves. También puedes duplicar una sección de grillas.

## Exportando reportes

 Haz click en el botón descargar para exportar tu reporte como un archivo zip LaTeX. Verifica README.md, en tu directorio descargado, para encontrar las instrucciones de cómo convertir este archivo a un PDF. Es fácil subir el archivo zip a [Overleaf](https://www.overleaf.com/) para editar LaTeX.

## Reportes cruzados del proyecto

Compara las ejecuciones de dos proyectos diferentes con los reportes cruzados del proyecto. Utiliza el selector del proyecto, en la tabla del conjunto de ejecuciones, para seleccionar un proyecto.

![](.gitbook/assets/how-to-pick-a-different-project-to-draw-runs-from.gif)

Las visualizaciones en la sección toman las columnas del primer conjunto de ejecuciones activo. Si no ves la métrica que estás buscando en el gráfico de líneas, asegúrate de que el primer conjunto de ejecuciones registrado en la sección tenga la columna disponible. Esta característica soporta datos del historial sobre las líneas de series de tiempo, pero no soportamos que se tomen diferentes métricas de la síntesis desde diferentes proyectos – así que un diagrama de dispersión no funcionaría para las columnas que solo se registraron en otro proyecto.

Si realmente necesitas comparar las ejecuciones de dos proyectos, y las columnas no están funcionando, agrega una etiqueta a las ejecuciones en un proyecto y entonces mueve estas ejecuciones al otro proyecto. Aún vas a ser capaz de filtrar sólo las ejecuciones de cada proyecto, pero tendrás disponibles todas las columnas para ambos conjuntos de ejecuciones en el reporte.

### Enlaces hacia el reporte de sólo lectura

Comparte un enlace de sólo lectura para un reporte que esté en un proyecto privado o en un proyecto de equipo.

![](.gitbook/assets/share-view-only-link.gif)

### Envía un gráfico a un reporte

Envía un gráfico desde tu entorno de trabajo a un reporte para hacer el seguimiento de tu progreso. Haz click en el menú desplegable sobre el gráfico o sobre el panel que te gustaría copiar al reporte, y haz click en **Agregar a reporte** para seleccionar el reporte destino.

![](.gitbook/assets/demo-export-to-existing-report%20%281%29%20%282%29%20%283%29%20%283%29%20%283%29%20%283%29%20%281%29.gif)

## Preguntas Frecuentes de los Reportes

### Sube un CSV a un reporte

 Si actualmente deseas subir un CSV a un reporte, puedes hacerlo a través del formato `wandb.Table`. Cargar el CSV en tu script de Python, y registrarlo como un objeto `wandb.Table`, te permitirá representar los datos como una tabla en un reporte.

### Refrescando datos

Recarga la página para refrescar los datos en un reporte y para obtener los últimos resultados de tus ejecuciones activas. Los entornos de trabajo automáticamente van a cargar **datos frescos**, si es que tienes activada la opción Refrescar automáticamente \(disponible en el menú desplegable, en la esquina superior derecha de tu página\). Refrescar automáticamente no se aplica a los reportes, así que estos datos no se van a refrescar hasta que no recargues la página.

