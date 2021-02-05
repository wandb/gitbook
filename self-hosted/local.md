---
description: Ejecuta Weights and Biases en tus propias máquinas utilizando Docker
---

# Local

## Comenzando el servidor

Para correr el servidor de W&B localmente, vas a necesitar tener instalado a [Docker](https://www.docker.com/products/docker-desktop). Entonces, simplemente ejecuta:

```text
wandb local
```

En segundo plano, la biblioteca cliente de wandb está corriendo la imagen [wandb/local](https://hub.docker.com/repository/docker/wandb/local) de docker, reenviando al puerto 8080 del host, y configurando a tu máquina para enviar métricas a tu instancia local en vez de a nuestro servidor en la nube. Si deseas correr nuestro contenedor local de forma manual, puedes ejecutar el siguiente comando de docker:

```text
docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

### Hosting Centralizado

Correr wandb en localhost es magnífico para el testeo inicial, pero para hacer uso de características colaborativas de wandb/local, deberías albergar el servicio en un servidor central. Las instrucciones para establecer un servidor centralizado en varias plataformas se pueden encontrar en la sección [Ajustes](https://docs.wandb.ai/self-hosted/setup).

###  Configuración Básica

 Al ejecutar `wandb loca`l se configura a tu máquina local para que envíe métricas a [http://localhost:8080](http://localhost:8080/). Si quieres usar un puerto diferente, puedes pasar el argumento `–port` a wandb local. Si has configurado DNS con tu instancia local puedes ejecutar: `wandb login --host=`[`http://wandb.myhost.com`](http://wandb.myhost.com/)en cualquier máquina desde donde quieras que se reporten métricas. También puedes establecer la variable de entorno `WANDB_BASE_URL` para un host o una IP sobre cualquier máquina que quieras que reporte a tu instancia local. En entornos automatizados, también vas a querer establecer la variable de entorno WANDB\_API\_KEY dentro de una clave de la api, desde tu página de ajustes. Para restituir una máquina para que reporte métricas a nuestra solución albergada en la nube, ejecuta `wandb login –host=`[`https://api.wandb.ai`](https://api.wandb.ai/).

### Autenticación

La instalación básica de wandb/local comienza con un usuario por defecto [local@wandb.com](mailto:local@wandb.com). La contraseña por defecto es perceptron. El frontend intentará iniciar sesión automáticamente con este usuario y te pedirá que restablezcas tu contraseña. Una versión de wandb sin licencia te va a permitir crear hasta 4 usuarios. Puedes configurar a los usuarios en la página Administración de Usuarios de wandb/local que se encuentra en [`http://localhost:8080/admin/users`](http://localhost:8080/admin/users).

### Persistencia y Escalabilidad

Todos los metadatos y los archivos enviados a W&B son almacenados en el directorio `/vol`. 

Si no montas un volumen persistente en esta ubicación, todos los datos se van a perder cuando el proceso de docker muera.Si compraste una licencia para wandb/local, puedes almacenar los metadatos en una base de datos MySQL externa, y los archivos en un bucket de almacenamiento externo, removiendo la necesidad de un contenedor con estado, así también como dándote las características de resiliencia y escalabilidad que típicamente son necesarias para las cargas de trabajo en producción.Mientras que W&B puede ser utilizado para hacer uso del volumen persistente montado en `/vol`, como se mencionó anteriormente, esta solución no es la deseada para las cargas de trabajo en producción. Si decides utilizar a W&B de esta forma, se recomienda que tengas suficiente espacio asignado de antemano, para almacenar las necesidades actuales y futuras de las métricas, y se sugiere fuertemente que el almacén de archivos subyacente pueda ser redimensionado cuando sea necesario. En adición, se deberían establecer alertas para permitirte saber una vez que los umbrales de almacenamiento mínimo se hayan cruzado, para redimensionar el sistema de archivos subyacente.Para hacer pruebas, recomendamos al menos 100GB de espacio libre en el volumen subyacente para cargas de trabajo pesadas que no sean imágenes/videos/audios. Si estás testeando a W&B con archivos grandes, el almacén precisa tener suficiente espacio para acomodar aquellas necesidades. En todos los casos, el espacio asignado necesita reflejar las métricas y las salidas de tus procesos de trabajo.

###  Actualizaciones

Regularmente, estamos subiendo nuevas versiones de wandb/local a dockerhub. Para actualizar puedes ejecutar:

```text
$ wandb local --upgrade
```

Para actualizar tu instancia manualmente, puedes ejecutar lo siguiente

```text
$ docker pull wandb/local
$ docker stop wandb-local
$ docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

### Obtener una licencia

Si estás interesado en configurar equipos, utilizando almacenamiento externo, o desplegando wandb/local a un cluster de Kubernetes, envíanos un correo electrónico a [contact@wandb.com](mailto:contact@wandb.com)

