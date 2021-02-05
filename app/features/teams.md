---
description: >-
  Colabora con tus colegas, comparte resultados, y haz un seguimiento de todos
  los experimentos pertenecientes a tu equipo
---

# Teams

Utiliza Weights & Biases como un repositorio central para tu equipo de aprendizaje de máquinas.

* Haz un seguimiento de todos los experimentos que tu equipo ha probado, así nunca tienes que duplicar el trabajo. Sólo algunas líneas de instrumentación te otorgan un rastreo rápido y legible de las métricas del desempeño del modelo, las predicciones, el uso de la GPU, y la versión del código que entrenó al modelo.
*  Gurda, restaura y reproduce los modelos previamente entrenados.
* **Comparte el progreso** y los resultados con tu jefe y con tus colaboradores.
* **Captura la regresión** y sé advertido de inmediato cuando el desempeño caiga.
* Desempeño del modelo de referencia y consultas personalizadas para comparar las versiones de tu modelo.

## Preguntas Comunes

### Obtén acceso a los equipos privados 

Si estás en una compañía, tenemos planes empresariales. Verifica la [página de ](https://www.wandb.com/pricing)[tarifas](https://www.wandb.com/pricing) para obtener más detalles. Ofrecemos equipos privados gratuitos para académicos trabajando en proyectos de código abierto. Verifica la [página de académicos](https://www.wandb.com/academic) para aplicar a una actualización. 

### Crea un nuevo equipo

Una vez que tengas la característica habilitada, crea un nuevo equipo en tu página [Ajustes](https://app.wandb.ai/settings) en la aplicación. El nombre va a ser usado en la URL de todos los proyectos de tu equipo, así que asegúrate de seleccionar algo corto y descriptivo, puesto que no vas a ser capaz de cambiarlo después.

###  Mueve las ejecuciones a un equipo

 Es fácil mover las ejecuciones entre los proyectos a los que tienes acceso. En la página del proyecto:

1. Haz click en la pestaña tabla para expandir la tabla de las ejecuciones
2. Haz click en la casilla de selección para seleccionar todas las ejecuciones
3. Haz click en Mover: el proyecto de destino puede estar en tu cuenta personal o en la de cualquier equipo del que seas miembro. ****

![](../../.gitbook/assets/demo-move-runs.gif)

### Envía nuevas ejecuciones a tu equipo

En tu script, establece la entidad para tu equipo. “Entidad” se refiere a tu nombre de usuario o al nombre de tu equipo. Crea una entidad \(cuenta personal o cuenta del equipo\) en la aplicación web antes de enviar las ejecuciones allí.

```python
wandb.init(entity="example-team")
```

Tu **entidad por defecto** se actualiza cuando te unes a un equipo. Esto significa que en tu [página de ajustes](https://app.wandb.ai/settings) vas a ver que la ubicación por defecto para crear un nuevo proyecto, ahora es la correspondiente al equipo al que te has terminado de unir. Aquí hay un ejemplo de cómo se ve la sección de la [página de ajustes](https://app.wandb.ai/settings):

![](../../.gitbook/assets/screen-shot-2020-08-17-at-12.48.57-am.png)

###  Valores Predeterminados del Proyecto

Ubicación por defecto para crear nuevos proyectos  
app.wandb.ai/teams/&lt;your-team-here&gt;

### See privacy settings

Debajo están los ajustes por defecto para los nuevos proyectos que vayas a crear en tu cuenta personal. Para ver los ajustes de tu organización, ve a tu página de los ajustes del equipo. 

La privacidad predeterminada del proyecto en tu cuenta personal                                        

PRIVADAHabilita el guardado de código en tu cuenta personalRegión del almacenamiento en la nube por defecto en tu cuenta personal

![](../../.gitbook/assets/demo-team-settings.png)

###  Mira los ajustes de la privacidad

Puedes ver los ajustes de la privacidad de todos los proyectos del equipo, desde la página de ajustes del equipo:app.wandb.ai/teams/&lt;your-team-here&gt;

### Tipos de Cuentas

Invita a los colegas a unirse al equipo, y selecciona de estas opciones:

* **Miembro**: Un miembro regular de tu equipo, invitado por correo electrónico. 
* **Administrador**: Un miembro del equipo que puede agregar y remover a otros administradores y miembros.. 
* **Servicio:** un proveedor de servicios, una clave de la API útil para usar W&B con tus herramientas de automatización de las ejecuciones. Si utilizas la clave de la API desde una cuenta de servicio para tu equipo, asegúrate de establecer la variable de entorno WANDB\_USERNAME para las ejecuciones del atributo al usuario correcto.

