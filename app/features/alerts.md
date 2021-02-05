---
description: >-
  Recibe una notificación de Slack toda vez que tu ejecución falla, se completa
  o llama a wandb.alert().
---

# Alerts

W&B puede enviar notificaciones a la casilla de correos electrónica o a Slack toda vez que tu ejecución falla, se completa o llama a [wandb.alert\(\)](https://docs.wandb.ai/library/wandb.alert).

###  Alertas de Usuario

Establece notificaciones cuando una ejecución finaliza, falla, o tú mismo llamas a `wandb.alert()`. Esto se aplica a todos los proyectos donde lanzas ejecuciones, incluyendo proyectos personales y proyectos en equipo.

En tus [Ajustes de Usuario](https://wandb.ai/settings):

*  Desplázate a la sección **Alertas**
* . Haz click en **Conectar a Slack** para seleccionar un canal al cual enviar alertas. Recomendamos el canal Slackbot, puesto que hace que las alertas sean privadas.
* **Email** irá a la dirección de correo electrónico que usaste cuando te registraste en W&B. Te recomendamos establecer un filtro en tu casilla de correos para que todas estas alertas vayan a un directorio y no te llenen la bandeja de entrada

![](../../.gitbook/assets/demo-connect-slack.png)

### Alertas

Los administradores del equipo pueden establecer alertas para el equipo en la página de ajustes del equipo: wandb.ai/teams/your-team. Estas alertas se aplican a todos los miembros del equipo. Recomendamos usar el canal Slackbot, porque mantiene las alertas privadas.

### Cambiando los Canales de Slack

**Desconectar Slack** y entonces en reconectar, seleccionando un canal de destino diferente.

