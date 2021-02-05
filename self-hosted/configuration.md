---
description: Cómo configurar la instalación del Servidor de W&B Local
---

# Advanced Configuration

Tu Servidor de W&B Local viene listo para usarse desde el arranque. Sin embargo, están disponibles varias opciones de configuraciones avanzadas en la página `/system-admin` de tu servidor, una vez que éste esté en marcha. Puedes enviar un correo electrónico a [contact@wandb.com](mailto:contact@wandb.com) para solicitar una licencia de prueba, para permitir más usuarios y equipos.

##  Configuración como código 

Todos los ajustes de la configuración pueden ser establecidos a través de la interfaz de usuario, sin embargo si quisieras administrar estas opciones de configuración a través del código, puedes establecer las siguientes variables de entorno:

| Variable de Entorno | Descripción |
| :--- | :--- |
| LICENSE | Tu licencia de wandb/local |
| MYSQL | El string de conexión a MySQL |
| BUCKET | El bucket S3 / GCS para almacenar datos |
| BUCKET\_QUEUE | La cola SQS / Google PubSub para los eventos de creación de objetos |
| NOTIFICATIONS\_QUEUE | La cola SQS en la que se publican los eventos de las ejecuciones |
| AWS\_REGION | La Región AWS donde recide tu bucket |
| HOST | El FQD de tu instancia, es decir, [https://my.domain.net](https://my.domain.net) |
| AUTH0\_DOMAIN | El dominio Auth0 de tu alquiler |
| AUTH0\_CLIENT\_ID | El ID del Cliente Auth0 de la aplicación |
| SLACK\_CLIENT\_ID | El ID del cliente de la aplicación Slack que quieres usar para las alertas |
| SLACK\_SECRET | El secreto de la aplicación Slack que quieres usar para las alertas |

## Autenticación

 Por defecto, un Servidor de W&B Local se ejecuta con administración de usuarios manual, permitiendo hasta 4 usuarios. La versiones de wandb/local con licencia también desbloquean SSO usando Auth0.

 Tu servidor soporta cualquier proveedor de autenticación soportado por [Auth0](https://auth0.com/). Deberías establecer tu propio dominio de Auth0 y la aplicación que va a estar bajo el control de tus equipos.

Después de crear una aplicación Auth0, vas a necesitar configurar a tus callbacks de Auth0 al el host de tu Servidor de W&B. Por defecto, el servidor soporta http desde las direcciones IP públicas o privadas por el host. También puedes configurar un nombre de host de DNS y un certificado SSL, si así lo eliges.

* Establece la URL del Callback a`http(s)://YOUR-W&B-SERVER-HOST`
* Establece el Origen Web Permitido a `http(s)://YOUR-W&B-SERVER-HOST`
* Establece la URL de Cierre de Sesión a `http(s)://YOUR-W&B-SERVER-HOST/logout`

![Ajustes de Auth0](../.gitbook/assets/auth0-1.png)

Guarda el ID del cliente y el dominio de tu aplicación Auth0.

![Ajustes de Auth0](../.gitbook/assets/auth0-2.png)

Entonces, navega a la página de ajustes de W&B en `http(s)://YOUR-W&B-SERVER-HOST/admin-settings`. Habilita la opción “Personalizar Autenticación con Auth0”, y completa el ID del Cliente y el dominio de tu aplicación Auth0.

![Ajustes de Autenticaci&#xF3;n Empresarial](../.gitbook/assets/enterprise-auth.png)

Finalmente, presiona “Actualiza los ajustes y reinicia W&B”.

##  Almacenamiento de archivos

 Por defecto, el Servidor Empresarial de W&B guarda los archivos a un disco de datos local con una capacidad que tu mismo estableces cuando aprovisionas a tu instancia. Para soportar un almacenamiento de archivos ilimitado, puedes configurar a tu servidor para que utilice un bucket de almacenamiento de archivos externo en la nube, con una API compatible con S3.

### Servicios Web de Amazon

Para utilizar un bucket S3 de AWS como el backend para el almacenamiento de archivos para W&B, vas a necesitar crear un bucket, conjuntamente con una cola SQS que esté configurada para recibir notificaciones de la creación de objetos desde dicho bucket. Tu instancia precisará permisos para leer desde la cola.

**Crea un cola SQS**

 Primero, crea una Cola Estándar SQS. Agrega un permiso para todos los principales para las acciones `SendMessage` y `ReceiveMessage`, así también como `GetQueueUrl`. \(Si quieres, puedes protegerlo aún más utilizando un documento de políticas avanzadas\).

![Ajustes de almacenamientos de archivos empresariales](../.gitbook/assets/sqs-perms.png)

**Crea un Bucket S3 y Notificaciones para el Bucket**

Entonces, crea un bucket S3. Bajo la página de propiedades del bucket en la consola, en la sección “Eventos” de los “Ajustes Avanzados”, haz click en “Agregar Notificación”, y configura todos los eventos de creación de objetos para que sean enviados a la Cola SQS que configuraste anteriormente.

![Ajustes de almacenamientos de archivos empresariales](../.gitbook/assets/s3-notification.png)

Habilita el acceso CORS: tu configuración de CORS debería verse como a continuación:

```markup
<?xml version="1.0" encoding="UTF-8"?>
<CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
<CORSRule>
    <AllowedOrigin>http://YOUR-W&B-SERVER-IP</AllowedOrigin>
    <AllowedMethod>GET</AllowedMethod>
    <AllowedMethod>PUT</AllowedMethod>
    <AllowedHeader>*</AllowedHeader>
</CORSRule>
</CORSConfiguration>
```

**Configura el Servidor de W&B**

Finalmente, navega a la página de ajustes de W&B en `http(s)://YOUR-W&B-SERVER-HOST/admin-settings`. Habilita la opción “Usar un backend de almacenamiento de archivos externo”, y completa el bucket s3, la región, y la cola SQS con el siguiente formato:

* **Bucket de Almacenamiento de Archivos:** `s3://<bucket-name>`
* **Región de Almacenamiento de Archivos**: `<region>`
* **Suscripción de Notificaciones**:`sqs://<queue-name>`

![Ajustes de almacenamientos de archivos de AWS](../.gitbook/assets/aws-filestore.png)

 Presiona “actualizar ajustes y reiniciar W&B” para aplicar los nuevos ajustes.

### Plataforma Google Cloud

Para usar el bucket de almacenamiento GCP como un backend de almacenamiento de archivos para W&B, vas a necesitar crear un bucket, conjuntamente con un tema y una suscripción pubsub configurados para recibir mensajes de creación de objetos desde dicho bucket.

**Crea Temas y Suscripciones Pubsub**

Navega a Pub/Sub &gt; Temas en la consola GCP, y haz click en “Crear tema”. Elige un nombre y crea un tema.

Entonces, haz click en “Crear suscripción” en la tabla de suscripciones, en la parte inferior de la página. Elige un nombre, y asegúrate de que el Tipo de Entrega sea establecido a “Pull”. Haz click en “Crear”.

Asegúrate de que la cuenta de servicio, o la cuenta en la que está corriendo tu instancia, tenga acceso a esta suscripción.

 **Crea un Bucket de Almacenamiento**

Navega hacia Almacenamiento &gt; Navegador en la Consola GCP, y haz click en “Crear bucket”. Asegúrate de elegir la clase de almacenamiento “Estándar”.

Asegúrate de que la cuenta de servicio, o la cuenta en la que está corriendo tu instancia, tenga acceso a este bucket.

 **Crea la Notificación Pubsub**

Desafortunadamente, crear un stream de notificaciones desde el Bucket de Almacenamiento al Tema Pubsub solamente puede ser realizado en la consola. Asegúrate de haber instalado a `gsutil`, y de que hayas registrado al Proyecto GCP correcto, entonces ejecuta lo siguiente:

```bash
gcloud pubsub topics list  # list names of topics for reference
gsutil ls                  # list names of buckets for reference

# create bucket notification
gsutil notification create -t <TOPIC-NAME> -f json gs://<BUCKET-NAME>
```

[  
Hay más referencias disponibles en el sitio web ](https://cloud.google.com/storage/docs/reporting-changes)[de ](https://cloud.google.com/storage/docs/reporting-changes)[Cloud Storage](https://cloud.google.com/storage/docs/reporting-changes)

**Agrega Permisos de Firma**

Para crear URLs de archivos firmados, tu instancia de W&B también necesita el permiso `iam.serviceAccounts.signBlob` en GCP. También puedes agregarlo al agregar el rol `Service Account Token Creator` a la cuenta de servicio, o a un miembro IAM que es como tu instancia está corriendo.

 **Otorga Permisos al Nodo Corriendo W&B**

 El nodo en el que tu W&B Local está corriendo debe estar configurado para permitir el acceso a s3 y sqs. Dependiendo del tipo de servidor desplegado por el que hayas optado, podrías necesitar agregar las siguientes declaraciones de políticas al rol de tu nodo:

```text
{
   "Statement":[
      {
         "Sid":"",
         "Effect":"Allow",
         "Action":"s3:*",
         "Resource":"arn:aws:s3:::<WANDB_BUCKET>"
      },
      {
         "Sid":"",
         "Effect":"Allow",
         "Action":[
            "sqs:*"
         ],
         "Resource":"arn:aws:sqs:<REGION>:<ACCOUNT>:<WANDB_QUEUE>"
      }
   ]
}
```

**Configura al Servidor W&B**

Finalmente, navega a la página de ajustes de W&B en `http(s)://YOUR-W&B-SERVER-HOST/admin-settings`. Habilita la opción “Utilizar el backend de almacenamiento de archivos externo”, y completa el bucket s3, la región y la cola SQS en el siguiente formato:

* **Bucket de Almacenamiento de Archivos**:`gs://<bucket-name>`
* **Región de Almacenamiento de Archivos:** blank
* **Suscripción de Notificaciones:** `pubsub:/<project-name>/<topic-name>/<subscription-name>`

![Ajustes del almacenamiento de archivos de GCP](../.gitbook/assets/gcloud-filestore.png)

 Presiona “actualizar ajustes y reinicar W&B” para aplicar los nuevos ajustes.

### Azure

Para usar el contenedor de blobs de Azure como el almacenamiento de archivos para W&B, vas a necesitar crear una cuenta de almacenamiento \(si no tienes ya una que quieras usar\), crear un contenedor de blobs y una cola con esa cuenta de almacenamiento, y entonces crea una suscripción de eventos que envíe notificaciones de “blob creado” a la cola desde el contenedor de blobs.

#### Crea una Cuenta de Almacenamiento

Si ya tienes una cuenta de almacenamiento que quieras usar, puedes saltear este paso.

Navega a [Cuentas de Almacenamiento &gt; Agregar](https://portal.azure.com/#create/Microsoft.StorageAccount) en el portal de Azure. Selecciona una suscripción de Azure, y selecciona cualquier grupo de recursos o crea uno nuevo. Ingresa un nombre para la cuenta de almacenamiento.

![Ajuste de la cuenta de almacenamiento de Azure](../.gitbook/assets/image%20%28106%29.png)

Haz click en Revisar y Crear y, entonces, en la pantalla del resumen, haz click en Crear.

![Revisi&#xF3;n de los detalles de la cuenta de almacenamiento de Azure](../.gitbook/assets/image%20%28114%29.png)

####  Crear el contenedor de blobs

 Ve a las [Cuentas de Almacenamiento](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Storage%2FStorageAccounts) en el portal de Azure, y haz click en tu nueva cuenta de almacenamiento. En el panel de control de tu cuenta de almacenamiento, haz click en Servicio de Blobs &gt; Contenedores, en el menú:

![](../.gitbook/assets/image%20%28102%29.png)

Crear el contenedor de blobs

![](../.gitbook/assets/image%20%28110%29.png)

Ve a Ajustes &gt; CORS &gt; Servicio de Blobs, e ingresa la IP de tu servidor wandb como un origen permitido, con los métodos permitidos `GET` y `PUT`, y todas las cabeceras permitidas y expuestas, entonces guarda tus ajustes CORS.

![](../.gitbook/assets/image%20%28119%29.png)

#### Creando la Cola

Ve a Servicio de colas &gt; Colas, en tu cuenta de almacenamiento, y crea una nueva Cola:

![](../.gitbook/assets/image%20%28101%29.png)

Ve a Eventos en tu cuenta de almacenamiento, y crea una suscripción a eventos:

![](../.gitbook/assets/image%20%28108%29.png)

Dale a la suscripción de eventos el Esquema de Eventos “Esquema de Grilla de Eventos”, filtra sólo al tipo de evento “Blob Creado”, establece el Tipo del Punto Final a Colas de Almacenamiento, y entonces selecciona la cuenta/cola de almacenamiento como el punto final.

![](../.gitbook/assets/image%20%28116%29.png)

En la pestaña Filtros, habilita el filtrado de temas para temas comenzado con`/blobServices/default/containers/your-blob-container-name/blobs/`

![](../.gitbook/assets/image%20%28105%29.png)

#### Configurar el Servidor de W&B

Ve a Ajustes &gt; Claves de acceso, en tu cuenta de almacenamiento, haz click en “Mostrar claves”, y entonces copia key1 &gt; Key o key2 &gt; Key. Establece esta clave en tu servidor de W&B como la variable de entorno de `ZURE_STORAGE_KEY`.

![](../.gitbook/assets/image%20%28115%29.png)

 Finalmente, navega a la página de ajustes de W&B en `http(s)://YOUR-W&B-SERVER-HOST/admin-settings`. Habilita la opción “Usar un backend de almacenamiento de archivos externo”, y completa el bucket s3, la región y la cola SQS en el siguiente formato:

* **Bucket de Almacenamiento de Archivos**: `az://<storage-account-name>/<blob-container-name>`
* S**uscripción de Notificaciones:** `az://<storage-account-name>/<queue-name>`

![](../.gitbook/assets/image%20%28109%29.png)

Presiona “Actualizar ajustes” para aplicar los nuevos ajustes.

## Slack

Para integrar tu instalación local de W&B con Slack, vas a necesitar crear una aplicación Slack apropiada.

#### Crear la aplicación Slack

 Visita [https://api.slack.com/apps](https://api.slack.com/apps) y selecciona **Crear Nueva Aplicación** en la esquina superior derecha.

![](../.gitbook/assets/image%20%28123%29.png)

Puedes nombrarla como lo desees, pero lo que es importante es seleccionar el mismo nombre del entorno de trabajo de Slack que el que pretendes utilizar para las alertas.

![](../.gitbook/assets/image%20%28124%29.png)

####  Configurando la aplicación Slack

Ahora que tenemos lista a la aplicación Slack, necesitamos autorización para usarla como un bot OAuth. Slecciona **OAuth & Permisos** en la barra lateral izquierda.

![](../.gitbook/assets/image%20%28125%29.png)

 Debajo de Alcances, provee al bot con el alcance **incoming\_webhook**.

![](../.gitbook/assets/image%20%28128%29%20%281%29.png)

Finalmente, configura la URL de Redirección para que apunte a tu instalación de W&B. Deberías usar el mismo valor que el que estableciste en Frontend Host en tus ajustes del sistema local. Puedes especificar múltiples URLs si tienes diferentes mapeos DNS para tu instancia.

![](../.gitbook/assets/image%20%28127%29.png)

 Una vez que esté finalizado, aprieta en Guardar URLs.

Para asegurar aún más a tu aplicación Slack y evitar el abuso, puedes especificar un rango de IPs debajo de **Restringir el Uso del Token** de la API, poniendo en una lista blanca la IP, o el rango de las IPs de tu\(s\) instancia\(s\) de W&B.

#### Registrar tu aplicación Slack con W&B

 Navega a la página de Ajustes del Sistema de tu instancia de W&B. Tilda la caja para habilitar una aplicación Slack personalizada:

![](../.gitbook/assets/image%20%28126%29.png)

Vas a necesitar proveer la ID del cliente y el secreto de tu aplicación Slack, que puedes encontrar en la pestaña Información Básica.

![](../.gitbook/assets/image%20%28120%29.png)

¡Eso es todo! Ahora puedes verificar que todo esté funcionando al establecer una integración en la aplicación de W&B. Visita [esta página](https://docs.wandb.ai/app/features/alerts) para obtener información más detallada.

