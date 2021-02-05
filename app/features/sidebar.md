---
description: Cómo usar la barra lateral y la tabla en la página del proyecto
---

# Table

En la página del proyecto, mostramos las ejecuciones en una barra lateral. Expande la barra lateral para ver una tabla de hiperparámetros y métricas de la síntesis a través de las ejecuciones.

##  Busca los nombres de las ejecuciones

 Soportamos íntegramente las búsquedas a través de [expresiones regulares](https://dev.mysql.com/doc/refman/8.0/en/regexp.html) sobre los nombres de las ejecuciones en la tabla. Cuando escribes una consulta en la caja de búsqueda, eso va a filtrar las ejecuciones visibles en loas gráficos del entorno de trabajo, así también como las filas de la tabla.

##  Redimensiona la barra lateral

 ¿Te gustaría hacer más espacio para los gráficos en la página del proyecto? Haz click y arrastra el borde de la cabecera de la columna para redimensionar a la barra lateral. Aún vas a ser capaz de hacer click en el ícono del ojo para activar o desactivar las ejecuciones en los gráficos. 

![](https://downloads.intercomcdn.com/i/o/153755378/d54ae70fb8155657a87545b1/howto+-+resize+column.gif)

## Agrega columnas a la barra lateral

En la página del proyecto, mostramos las ejecuciones en una barra lateral. Para mostrar más columnas:

1. Haz click en el botón de la esquina superior derecha de la barra lateral para expandir la tabla.
2. En una cabecera de columna, haz click en el menú desplegable para fijar una columna.
3. Las columnas fijadas van a estar disponibles en la barra lateral cuando pliegues la tabla.

Aquí hay una captura de pantalla. Expando la tabla, fijo dos columnas, pliego la tabla, y entonces redimensiono la barra lateral.

![](https://downloads.intercomcdn.com/i/o/152951680/cf8cbc6b35e923be2551ba20/howto+-+pin+rows+in+table.gif)

##  Selecciona en masa a las ejecuciones

Borra múltiples ejecuciones al mismo tiempo, o etiqueta a un grupo de ejecuciones – la selección en masa facilita la organización de la tabla.

![](../../.gitbook/assets/howto-bulk-select.gif)

## Selecciona a todas las ejecuciones en la tabla

Haz click en la casilla de selección, en la esquina superior izquierda de la tabla, y haz click en “Selecciona todas las ejecuciones” para seleccionar cada ejecución que se corresponda con el conjunto de filtros actuales.

![](../../.gitbook/assets/all-runs-select.gif)

## Mueve las ejecuciones entre los proyectos

Para mover las ejecuciones de un proyecto a otro:

1. Expande la tabla
2. Haz click en la casilla de selección al lado de las ejecuciones que deseas mover
3. Haz click en mover y selecciona el proyecto destino

![](../../.gitbook/assets/howto-move-runs.gif)

## Mira las ejecuciones activas

Busca un punto verde al lado del nombre de las ejecuciones – esto indica que están activas en la tabla y en las leyendas del gráfico.

## Oculta las ejecuciones que no sean interesantes

  ¿Deseas ocultar las ejecuciones que fallaron? ¿Las ejecuciones cortas están llenando tu tabla? ¿Sólo quieres ver tu trabajo en un proyecto grupal? Oculta el ruido con un filtro. Algunos filtros que recomendamos:

* **Muestra solamente mi trabajo,** filtra las ejecuciones según tu nombre de usuario
* **Oculta las ejecuciones fallidas**, saca de la tabla cualquier ejecución marcada como fallida
* **Duración:** agrega un nuevo filtro y selecciona “duración” para ocultar a las ejecuciones cortas.

![](../../.gitbook/assets/image%20%2816%29.png)

##  Filtra y borra a las ejecuciones no deseadas

Si filtras la tabla a sólo aquellas a las que quieras borrar, puedes seleccionarlas a todas y presionar borrar para removerlas de tu proyecto. El borrado de ejecuciones es global al proyecto, así que si borras las ejecuciones de un reporte, eso se verá reflejado en el resto de tu proyecto.

![](../../.gitbook/assets/2020-05-13-19.14.13.gif)

## Exporta ejecuciones a CSV

Exporta la tabla de todas tus ejecuciones, hiperparámetros y métricas de síntesis a un CSV con el botón descargar.

![](../../.gitbook/assets/2020-07-06-11.51.01.gif)

