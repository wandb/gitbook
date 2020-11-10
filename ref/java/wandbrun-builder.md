---
description: >-
  The Builder is used to help configure and initialize a run before the training
  loop where you log model metrics.
---

# WandbRun.Builder

### Overview

The Builder pattern allows us to write readable code to set up a WandbRun. The builder contains a few functions used to help initialize these values.

* **builder.build\(\)** — returns a WandbRun instance, representing a run 
* **builder.withName\(String name\)** — a display name for this run, which shows up in the UI and is editable, doesn't have to be unique
* **builder.withConfig\(JSONObject data\)** — a Java JSON Object that contains any initial config values
* **builder.withProject\(String project\)** — the name of the project to which this run will belong
* **builder.withNotes\(String notes\)** — a description associated with the run
* **builder.setTags\(List&lt;String&gt; tags\)** — an array of tags to be used with the run
* **builder.setJobType\(String type\)** — the type of job you are logging, e.g. eval, worker, ps \(_default: training_\)
* **builder.withGroup\(String group\)** — a string by which to group other runs; see [Grouping](../../library/grouping.md)

Most of these settings can also be controlled via [Environment Variables](../../library/environment-variables.md). This is often useful when you're running jobs on a cluster.

### Examples

Initializing a default run

```java
WandbRun run = new WandbRun.Builder().build();
```

Initializing a run with a config object and name

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

 



