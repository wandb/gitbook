---
description: >-
  Compara las versiones de tu modelo, explora los resultados en un entorno de
  trabajo temporal, y exporta las conclusiones a un reporte para guardar notas y
  visualizaciones.
---

# Project Page

El proyecto Entorno de Trabajo te ofrece un entorno de pruebas personal para comparar los experimentos. Utiliza los proyectos para organizar los modelos que pueden ser comparados, trabajando sobre el mismo problema con diferentes arquitecturas, hiperparámetros, conjuntos de datos, preprocesamientos, etc.

 Etiquetas de la página del proyecto:

1. \*\*\*\*[**Resumen**](https://docs.wandb.ai/app/pages/project-page#overview-tab)**:** Panorámica de tu proyecto.
2.  ****[**Entorno de Trabajo**](https://docs.wandb.ai/app/pages/project-page#workspace-tab)**:** entorno de pruebas personal para las visualizaciones
3.  ****[**Tabl**](https://docs.wandb.ai/app/pages/project-page#table-tab)\*\*\*\*[**a**](https://docs.wandb.ai/app/pages/project-page#table-tab)**:** Vista general de todas las ejecuciones
4.  ****[**Reportes**](https://docs.wandb.ai/app/pages/project-page#reports-tab)**:** Panorámicas guardadas de las notas, las ejecuciones y los gráficos
5.  ****[**Barridos**](https://docs.wandb.ai/app/pages/project-page#sweeps-tab)**:** Exploración automatizada y optimización

## Pestaña Resumen

* **Nombre del proyecto: haz click para editar el nombre del proyecto**
* **Descripción del proyecto: haz click para editar la descripción del proyecto y para agregar notas.** 
* **Borrar proyecto: haz click en el menú de puntos, en la esquina derecha, para borrar un proyecto.**
* **Privacidad del proyecto: edita quién puede ver las ejecuciones y los reportes – haz click en el ícono del candado.**
*  **Último activo: mira cuándo fueron registrados los datos más recientes para este proyecto.** 
* **Cómputo total: Sumamos todos los tiempos de ejecución, correspondientes al proyecto, para obtener este total.** 
* **Recuperar ejecuciones: Haz click en el menú desplegable y entonces en “Recuperar todas las ejecuciones” para recuperar las ejecuciones borradas de tu proyecto.**

 [Ver un ejemplo en tiempo real →](https://app.wandb.ai/example-team/sweep-demo/overview)

![](../../.gitbook/assets/image%20%2829%29%20%281%29%20%282%29%20%284%29%20%281%29.png)

![](../../.gitbook/assets/undelete.png)

## Pestaña Entorno de Trabajo

**Barra Lateral de las Ejecuciones:** lista de todas las ejecuciones en tu proyecto

* Menú de puntos: pasa el cursor del mouse encima de una fila de la barra lateral y vas a ver que a la izquierda aparece un menú. 
* Utiliza este menú para renombrar una ejecución, borrarla o detener una activa.. Ícono de visibilidad: haz click en el ojo para activar o desactivar las ejecuciones en los gráficos.. 
* Color: cambia el color de la ejecución a otro color de los que tenemos predeterminados, o a uno personalizado.. 
* Búsqueda: busca a las ejecuciones por nombre. Esto también filtra a las ejecuciones visibles en los diagramas.. 
* Filtro: utiliza el filtro de la barra lateral para reducir el conjunto de ejecuciones visibles.. 
* Grupo: selecciona una columna de la configuración para agrupar dinámicamente a tus ejecuciones, por ejemplo por arquitectura. El agrupamiento hace que los diagramas se muestren con una línea a lo largo del valor de la media, y una región sombreada para la varianza de los puntos en el gráfico..
* Ordenar: selecciona un valor por el cuál ordenar las ejecuciones, por ejemplo teniendo en cuenta la pérdida más baja o la precisión más alta. El orden afectará a las ejecuciones que se muestren en los gráficos.. 
* Botón expandir: expande la barra lateral en una tabla completa.. 
* Cuenta de las ejecuciones: el número en paréntesis, en la parte superior, es el número total de ejecuciones en el proyecto. El número \(visualizado como N\), es el número de ejecuciones que tienen el ojo abierto y están disponibles para ser visualizadas en cada diagrama. En el ejemplo de abajo, los gráficos sólo están mostrando las primeras 10 de las 183 ejecuciones. Edita un gráfico para incrementar el número máximo de ejecuciones visibles.

  **Diseño de los paneles:** utiliza este espacio temporal para explorar resultados, agregar y remover gráficos, y compara las versiones de tus modelos en base a diferentes métricas.

[Ver un ejemplo en tiempo real →](https://app.wandb.ai/example-team/sweep-demo)

![](../../.gitbook/assets/image%20%2838%29%20%282%29%20%283%29%20%282%29.png)

###  Busca ejecuciones

 Busca una ejecución por su nombre en la barra lateral. Puedes usar expresiones regulares para filtrar a tus ejecuciones visibles. La caja de búsqueda afecta qué ejecuciones son mostradas en el gráfico. Aquí hay un ejemplo:

![](../../.gitbook/assets/2020-02-21-13.51.26.gif)

### Agrega una sección de paneles

Haz click en el menú desplegable de la sección y entonces en “Agregar sección” para crear una nueva sección para los paneles. Puedes renombrar las secciones, arrastrarlas para reorganizarlas, y expandirlas y colapsarlas.

Cada sección tiene opciones en la esquina superior derecha:

* **Cambia a un diseño personalizado:** El diseño personalizado te permite redimensionar los paneles de forma individual.. 
* **Cambia a un diseño estándar**: El diseño estándar te permite redimensionar a la vez a todos los paneles de la sección, y te ofrece paginación.. 
* **Agrega una sección:** Agrega una sección por encima o por debajo del menú desplegable, o haz click en el botón, en la parte inferior de la página, para agregar una nueva sección.. 
* **Renombra la sección**: Cambia el título para tu sección.. 
* **Exporta la sección a un reporte**: Guarda esta sección de paneles a un nuevo reporte.. 
* **Borra la sección:** Elimina a toda la sección y a todos sus gráficos. Esto se puede deshacer con el botón ‘deshacer’, en la parte inferior de la página, en la barra del entorno de trabajo..
* **Agrega un panel:** Haz click en el botón más para agregar un panel a esta sección. 

![](../../.gitbook/assets/add-section.gif)

###  Mueve los paneles entre las secciones

Arrastra y suelta a los paneles para reordenar y organizar las secciones. También puedes hacer click en el botón “Mover”, en la esquina superior derecha de un panel, para seleccionar una sección a la que habría que mover el panel.

![](../../.gitbook/assets/move-panel.gif)

### Redimensiona paneles

* **Diseño estándar:** Todos los paneles mantienen el mismo tamaño, y hay páginas de paneles. Puedes redimensionar a los paneles al hacer click y arrastrar desde la esquina inferior derecha. Redimensiona la sección al hacer click y arrastrar desde la esquina inferior derecha de la sección.
* **Diseño personalizado**: todos los paneles son dimensionados de forma individual, y no hay páginas.

![](../../.gitbook/assets/resize-panel.gif)

###  Busca por métricas

Utiliza la caja de búsquedas, en el entorno de trabajo, para filtrar los paneles. Esta búsqueda coincide con los títulos de los paneles que, por defecto, son el nombre de las métricas visualizadas.

![](../../.gitbook/assets/search-in-the-workspace.png)

## Pestaña Tabla

Utiliza la tabla para filtra, agrupar y ordenar tus resultados.

[Ver un ejemplo en tiempo real →](https://app.wandb.ai/example-team/sweep-demo/table?workspace=user-carey)

![](../../.gitbook/assets/image%20%2886%29.png)

## Pestaña Reportes

Mira todas las panorámicas de los resultados en un lugar, y comparte las conclusiones con tu equipo.

![](../../.gitbook/assets/reports-tab.png)

## Pestaña Barridos

Comienza un nuevo barrido de tu proyecto.

![](../../.gitbook/assets/sweeps-tab.png)

## Preguntas Comunes

### Reinicia el Entorno de Trabajo

Si ves un error como el siguiente en la página de tu proyecto, esto es lo que hay que hacer para reiniciar tu entorno de trabajo.`"objconv: "100000000000" overflows the maximum values of a signed 64 bits integer"`

Agrega **?workspace=clear** al final de la URL y presiona la tecla enter. Esto debería llevarte a una versión limpia del entorno de trabajo de tu página del proyecto.

### Borra Proyectos

 Puedes borrar tu proyecto desde la pestaña Resumen, al hacer click sobre los tres puntos que están a la derecha.

![](../../.gitbook/assets/howto-delete-project.gif)

### Ajustes de Privacidad

Haz click en el candado, en la barra de navegación, en la parte superior de la página, para cambiar los ajustes de la privacidad del proyecto. Puedes editar quién puede ver o emitir ejecuciones a tu proyecto. Estos ajustes incluyen todas las ejecuciones y los reportes del proyecto. Si quisieras compartir tus resultados con algunas personas, puedes crear un [equipo privado](https://docs.wandb.ai/app/features/teams).

![](../../.gitbook/assets/image%20%2879%29.png)

### Borra un proyecto vacío

Borra un proyecto sin ejecuciones al hacer click en el menú desplegable y al seleccionar “Borrar proyecto”.

![](../../.gitbook/assets/image%20%2866%29.png)

