# Data and Privacy

##  Tú posees tus datos

Todo lo que registras a Weights and Biases es tuyo, incluyendo tus datos de entrenamiento, el código, la configuración y los hiperparámetros, las métricas de salida, el análisis, y los archivos guardados del modelo. Puedes elegir registrar, exportar, publicar o borrar cualquiera de los mismos. Recogemos estadísticas globales entre nuestros usuarios para mejorar nuestro producto – podríamos hacer una consulta a la base de datos para contar cuántos usuarios han subido un requirements.txt, que incluya una biblioteca específica, para ayudar en la toma de decisiones de si queremos hacer una integración de primera clase con dicha biblioteca. Tratamos a tus datos privados, a tu código fuente o a los secretos comerciales como confidenciales y privados, en concordancia con nuestros [Términos del Servicio](https://www.wandb.com/terms) y [Política de Privacidad](https://www.wandb.com/privacy).‌

## Registro de datos

Nuestra herramienta provee la capacidad de registrar 4 clases de datos principales:

1. **Metrics and Parameters**: \_\*\*\_Esta es la funcionalidad central de la herramienta – hacer el seguimiento de escalares e histogramas que registras con una ejecución. Los especificas directamente en `wandb.log()` o estableces una integración con alguno de los frameworks soportados.
2. **Código:** Soportamos el guardado del último SHA de git y un parche diff, o el guardado del archivo principal de tu ejecución, para poder comparar el código fácilmente. Por defecto, esto está deshabilitado, y necesita ser habilitado manualmente desde tu [página de ajustes](https://app.wandb.ai/settings).
3. **Medios:** Los usuarios pueden registrar videos, imágenes, texto, o gráficos personalizados para visualizar cómo le está yendo a su modelo con los ejemplos durante el entrenamiento. Esto es totalmente optativo, y debes configurar explícitamente a tu script para registrar esta clase de datos.
4. **Artefactos:** Establece manualmente el registro de artefactos para guardar y versionar conjuntos de datos y archivos del modelo. Explícitamente, especificas qué archivos quieres incluir en los artefactos.

 En nuestro ofrecimiento en la nube, todos los datos se almacenan de forma encriptada en reposo y son encriptados cuando están en tránsito. Respetamos todas las solicitudes de retiro en una forma oportuna, y podemos asegurar que los datos serán erradicados del sistema.

## Auto Hospedaje y nube privada

Seguimos las mejores prácticas industriales para la seguridad y la encriptación en nuestro servicio de alojamiento en la nube. También ofrecemos [instalaciones en nubes privadas y auto hospedajes](https://docs.wandb.ai/self-hosted) para los clientes empresariales.

 [Comunícate con nosotros](https://docs.wandb.ai/company/getting-help) para aprender acerca de las opciones para tu negocio.Para uso personal, tenemos una [instalación Docker local](https://docs.wandb.ai/self-hosted/local) que puedes correr en tu propia máquina.‌

## Privacidad de los Proyectos y Equipos

Por defecto, los proyectos de Weights and Biases son privados, lo que significa que otros usuarios no van a ser capaces de ver tu trabajo. Puedes editar este valor predeterminado en tu [página de ajustes](https://app.wandb.ai/settings). Puedes elegir compartir tus resultados con otros al hacer que los proyectos sean públicos, o al crear un equipo para compartir proyectos privados con colaboradores específicos. Los equipos son una característica premium para las compañías. Aprende más en nuestra [página de tarifas](https://www.wandb.com/pricing).

Para apoyar al ecosistema de ML, ofrecemos equipos privados a los académicos y a los proyectos de código abierto. Regístrate para obtener una cuenta y entonces comunícate con nosotros a través de [este formulario](https://www.wandb.com/academic) para solicitar un equipo privado gratuito.

## Guardado de código

Por defecto, solamente tomamos el último SHA del git para tu código. Opcionalmente, puedes activar las características de guardado de código – esto va a habilitar un panel de comparación de código y una pestaña en la interfaz de usuario para ver la versión del código que corrió tu ejecución. Puedes activar el guardado de código en tu [página de ajustes](https://app.wandb.ai/settings).

![](../.gitbook/assets/project-defaults.png)

## Exportando datos.

 Puedes descargar los datos guardados con Weights & Biases utilizando nuestra [API de exportación](https://docs.wandb.ai/ref/export-api). Queremos que te sea fácil hacer el análisis personalizado en las notebooks, respaldar tus datos en el caso en el que quieras tener una copia local, o conectar tus registros guardados en otras herramientas en tu proceso de trabajo de ML.

## Cuentas enlazadas

Si utilizas OAuth en Google o GitHub para crear o iniciar sesión en una cuenta de Weights and Biases, no vamos a leer o a sincronizar los datos desde tus repositorios o directorios. Estas conexiones son puramente para propósitos de autenticación. Puedes registrar archivos y código para asociarlos con tus ejecuciones utilizando [Artefactos](https://docs.wandb.ai/artifacts) de W&B.

