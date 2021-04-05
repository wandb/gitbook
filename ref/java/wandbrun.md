---
description: L’objet WandbRun représente une instance d’un essai dans Java.
---

# WandbRun

###  Vue d’ensemble

Un essai peut être créé en utilisant le [WandbRun Builder](https://docs.wandb.ai/ref/java/wandbrun-builder). Cet objet est utilisé pour surveiller les essais 

* **run.log\(JSONObject data\)** — enregistre les données pour un essai, équivalent à [wandb.log\(\)](https://docs.wandb.ai/library/log)
* **run.log\(int step, JSONObject data\)** — enregistre les données pour un essai, équivalent à [wandb.log\(\)](https://docs.wandb.ai/library/log) à une étape spécifique
* **run.finish\(int exitCode\)** — finit un essai avec un code de sortie \(par défaut : 0\)

### Exemples

Tracer un signal sinusoïdal avec le client Java

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







