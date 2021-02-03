---
description: Límites apropiados y guías para registrar datos en Weights & Biases
---

# Limits

###  Mejores Prácticas para la Carga Rápida de Páginas

 Para la carga rápida de páginas en la Interfaz de Usuario de W&B, recomendamos mantener las cantidades de datos regsitrados dentro de estos límites.

* **Escalares:** puedes tener decenas de miles de pasos y cientos de métricas
* **Histogramas:** recomendamos que te limites a miles de pasos

 Si nos envías más que eso, tus datos serán guardados y se les hará un seguimiento, pero puede que las páginas se carguen más lentamente.

### Desempeño del Script de Python

Por lo general, no deberías estar llamando a `wandb.log` más que algunas pocas ve ces por segundo, o sino wandb puede comenzar a interferir con el desempeño de tu ejecución de entrenamiento. No establecemos ningún límite más allá de limitar la frecuencia. Nuestro cliente Python automáticamente hará un retraso exponencial y reintentará las solicitudes que excedan los límites, así que esto debería ser transparente para ti. Se verá “Network failure” en la línea de comandos. Para cuentas que no sean pagas, podríamos comunicarnos en casos extremos donde el uso exceda los umbrales razonables.

### Límites de frecuencia

La API de W&B tiene una frecuencia limitada por IP y por clave de la API. Las nuevas cuentas están restringidas a 200 solicitudes por minuto. Esta frecuencia te permite correr aproximadamente 15 procesos en paralelo y hacerlos reportar sin afectar al desempeño. Si el cliente `wandb detecta` que está siendo limitado, se retrasará y reintentará el envío de datos en el futuro. Si necesitas correr más de 15 procesos en paralelo, envía un email a [contact@wandb.com](mailto:contact@wandb.com).

###  Límites de los tamaños

#### Archivos

El tamaño máximo de archivos para las cuentas nuevas es de 2GB. Se permite que una ejecución simple almacene 10GB de datos. Si necesitas almacenar archivos más grandes, o almacenar más datos por ejecución, contáctanos a través de [contact@wandb.com](mailto:contact@wandb.com).

#### Métricas

Las métricas son muestreadas a 1500 puntos de datos, por defecto, antes de visualizarlas en la Interfaz de Usuario.

#### Registros

Mientras una ejecución está en progreso, tomamos las últimas 5000 líneas de tu registro para que las veas en la Interfaz de Usuario. Después de que la ejecución se completa, se archiva el registro entero, y este puede ser descargado desde una página de ejecución individual.

### Guías para el Registro

Aquí hay algunas guías adicionales para registrar datos a W&B

* **Parámetros anidados:** Automáticamente aplanamos a los parámetros anidados, así que si nos pasas un diccionario, lo convertiremos en un nombre separado por puntos. Para valores de configuración, soportamos 3 puntos en el nombre. Para valores de la síntesis, soportamos 4 puntos.

