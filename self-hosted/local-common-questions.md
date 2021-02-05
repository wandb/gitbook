---
description: >-
  Preguntas realizadas de forma frecuente acerca de establecer versiones de
  nuestra aplicación alojadas localmente.
---

# Local FAQ

## ¿Mi servidor necesita una conexión a internet?

No, wandb/local puede ejecutarse en entornos aislados. El único requerimiento es que las máquinas sobre las que se entrenan tus modelos puedan conectarse a este servidor a través de la red.

##  ¿Dónde son almacenados mis datos?

 La imagen de docker predeterminada corre a MySQL y a Minio dentro del contenedor, y escribe todos los datos en los subdirectorios de /vol. Puedes configurar el Almacenamiento externo de Objetos y de MySQL al obtener una licencia. Envía un correo electrónico a [contact@wandb.com](mailto:contact@wandb.com) para obtener más detalles.

##  ¿Cada cuánto liberan actualizaciones?

Nos esforzamos para liberar versiones actualizadas de nuestro servidor al menos una vez al mes.

##  ¿Qué pasa si mi servidor se cae?

Los experimentos que están en progreso van a ingresar en un ciclo de retroceso-reintento, y seguirá intentando conectarse durante 24 horas.

##  ¿Cuáles son la características para escalar a este servicio?

Una instancia simple de wandb/local, sin un almacenamiento externo de MySQL, va a escalar hasta 10 experimentos concurrentes, que van a ser rastreados a la vez. Las instancias conectadas a un almacenamiento externo de MySQL van a escalar hasta 100 ejecuciones concurrentes. Si tienes la necesidad de rastrear más experimentos concurrentes, envíanos una nota a [contact@wandb.com](mailto:contact@wandb.com) para consultar acerca de nuestras opciones de instalación para múltiples instancias con alta disponibilidad.

## ¿Cómo hago un restablecimiento de fábrica si no puedo acceder a mi instancia?

Si eres incapaz de conectarte a tu instancia, puedes ponerla en modo restitución al establecer la variable de entorno LOCAL\_RESTORE cuando inicias el entorno local. Si estás iniciando a wandb local a través de nuestra cli, puedes hacerlo con `wandb local -e LOCAL_RESTORE=true`. Mira los registros impresos en el arranque en busca de un nombre de usuario / contraseña temporal para acceder a la instancia.

## ¿Cómo puedo regresar a la nube después de usar el entorno local?

Para hacer que una máquina vuelva a reportar métricas a nuestra solución albergada en la nube, ejecuta `wandb login --host=https://api.wandb.ai`.

