---
description: WandbRun object represents an instance of a run in Java.
---

# WandbRun

### Overview

A run can be created by using the [WandbRun Builder](wandbrun-builder.md). This object is used to track runs  

* **run.log\(JSONObject data\)** — logs data for a run, equivalent to [wand.log\(\)](../../library/log.md)
* **run.log\(int step, JSONObject data\)** — logs data for a run, equivalent to [wand.log\(\)](../../library/log.md) at a specific step
* **run.finish\(int exitCode\)** — finishes a run with an exit code \(_default: 0_\)

### Examples

Plotting a sin wave with the Java client

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







