---
description: >-
  El Builder es utilizado para ayudar a configurar y a inicializar una ejecución
  antes del ciclo de entrenamiento, en donde registras las métricas del modelo.
---

# WandbRun.Builder

### Resumen

El patrón Builder nos permite escribir código legible para establecer un WandbRun. El builder contiene algunas funciones utilizadas para ayudar a inicializar estos valores.

* builder.build\(\) - devuelve una instancia de WandbRun, que representa una ejecución
* builder.withName\(String name\) – un nombre para visualizar esta ejecución, que se muestra en la Interfaz de Usuario y es editable, no tiene que ser único
*  builder.withConfig\(JSONObject data\) – un objeto JSON  de Java que contiene cualquier valor de configuración inicial
*  builder.withProject\(String project\) – el nombre del proyecto al que va a pertenecer esta ejecución
* builder.withNotes\(String notes\) – una descripción asociada con la ejecución
* builder.setTags\(List&lt;String&gt; tags\) – un arreglo de etiquetas que se va a usar con la ejecución
* builder.setJobType\(String type\) – el tipo de trabajo que estás registrando, por ejemplo, eval, worker, ps \(por defecto es training\)
* builder.withGroup\(String group\) – un string por el que se agrupa a otras ejecuciones; ver [Agrupamiento](https://docs.wandb.ai/library/grouping)

La mayoría de estos ajustes también pueden ser controlados a través de las [Variables de Entorno](https://docs.wandb.ai/library/environment-variables). Por lo general, esto es útil cuando estés ejecutando trabajos en un cluster.

### Ejemplos

Inicializar una ejecución por defecto

```java
WandbRun run = new WandbRun.Builder().build();
```

Inicializar una ejecución con un objeto de configuración y un nombre

```java
// Create JSONObject config
JSONObject config = new JSONOBject();
config.add("property", true);

// Use builder to customize run options
WandbRun run = new WandbRun.Builder()
    .withConfig(config)
    .withName("A Java Run")
    .build();
```

 



