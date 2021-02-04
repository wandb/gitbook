---
description: El objeto WandbRun representa una instancia de una ejecución en Java.
---

# WandbRun

### Resumen

 Una ejecución puede ser creada al usar el [WandbRun Builder](https://docs.wandb.ai/ref/java/wandbrun-builder). Este objeto es utilizado para rastrear ejecuciones:

* run.logrun.log\(JSONObject data\) – registra datos para una ejecución, es equivalente a [wand.log](https://docs.wandb.ai/library/log)[\(\)](https://docs.wandb.ai/library/log)
*  run.log\(int step, JSONObject data\) – registra datos para una ejecución, es equivalente a [wand.log](https://docs.wandb.ai/library/log)[\(\)](https://docs.wandb.ai/library/log) en un paso específico
* run.finish\(int exitCode\) – finaliza una ejecución con un código de salida \(por defecto: 0\)

### Ejemplos

Grafica una curva de seno con el cliente de Java

```java
// Initalize a run
WandbRun run = new WandbRun.Builder().build();

// Compute and log each sin value
for (double i = 0.0; i < 2 * Math.PI; i += 0.1) {
    JSONObject data = new JSONObject();
    data.put("value", Math.sin(i));
    run.log(data);
}

// Exit run when finished.
run.done();
```







