---
description: >-
  Ce Builder est utilisé pour aider à configurer et initialiser un essai avant
  la boucle d’entraînement lorsque vous enregistrez les mesures de modèle.
---

# WandbRun.Builder

### Vue d’ensemble

 Le schéma du Builder nous permet d’écrire du code lisible pour mettre en place un WandbRun. Ce builder contient quelques fonctions utilisées pour aider à initialiser ces valeurs.

* **builder.build\(\)** — renvoie une instance WandbRun, représentant un essai \(run\)
* **builder.withName\(String name\)** — un nom d’affichage pour cet essai, qui se retrouve dans l’IU et est modifiable, n’a pas besoin d’être unique
* **builder.withConfig\(JSONObject data\)** — un Objet JSON Java qui contient toute valeur de config initiale
* **builder.withProject\(String project\)** — le nom du projet auquel appartient cet essai
* **builder.withNotes\(String notes\)** — une description associée avec cet essai
* **builder.setTags\(List&lt;String&gt; tags\)** —un array d’étiquettes \(tags\) à utiliser dans cet essai
* **builder.setJobType\(String type\)** — le type de tache que vous enregistrez, e.g. eval, worker, ps \(par défaut : entraînement\)
* **builder.withGroup\(String group\)** — une chaîne de données avec laquelle regrouper les autres essais ; voir [Regroupements](https://docs.wandb.ai/library/grouping)

La plupart de ces paramètres peuvent être contrôlés par les [Variables d’Environnement](https://docs.wandb.ai/library/environment-variables). Souvent utile lorsque vous effectuez des taches sur un cluster.

###  Exemples

 Initialiser un essai par défaut

```java
WandbRun run = new WandbRun.Builder().build();
```

Initialiser un essai avec un objet config et un nom

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

 



