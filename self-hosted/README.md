---
description: Instalaciones de Auto Hospedaje para proyectos con datos sensibles
---

# Self Hosted

W&B Local es una versión de auto hospedaje de [W](https://app.wandb.ai/)[eights & Biases](https://app.wandb.ai/). Hace posible el seguimiento de experimentos colaborativos para equipos empresariales de aprendizaje de maquinas, dándote una forma de mantener a todos los datos y a los metadatos del entrenamiento dentro de la red de tu organización.

[Solicita una demostración para probar W&B Local →](https://www.wandb.com/demo)

 También ofrecemos [W&B Enterprise Cloud](https://docs.wandb.ai/self-hosted/cloud), que ejecuta una infraestructura completamente estable dentro de la cuenta AWS o GCP de tu compañía. Este sistema puede escalar a cualquier nivel de uso.

## Características

*  Ejecuciones, experimentos y reportes ilimitados
* Mantén tus datos seguros en la red tu propia compañía
* Integralo con el sistema de autenticación de tu compañía
* Soporte principal del equipo de ingenieros de W&B

El servidor del auto hospedaje es una imagen de Docker simple que es fácil de desplegar. Tus datos de W&B son guardados en un volumen persistente o en una base de datos externa, así los mismos pueden ser preservados entre las versiones del contenedor.

##  Requerimientos del Servidor

El servidor del auto hospedaje requiere una instancia con al menos 4 núcleos y 8GB de memoria.

##  Recursos del Auto Hospedaje

{% page-ref page="local.md" %}

{% page-ref page="setup.md" %}

{% page-ref page="configuration.md" %}

{% page-ref page="local-common-questions.md" %}

{% page-ref page="cloud.md" %}

