---
description: WandbRunオブジェクトは、Javaでの実行のインスタンスを表します。
---

# WandbRun

## 概要

runは、WandbRunBuilderを使用して作成できます。このオブジェクトは、実行を追跡するために使用されます·      

 **run.log\(JSONObject data\)** — logs data for a run, equivalent to [wand.log\(\)](https://docs.wandb.com/library/log)​·      

 **run.log\(int step, JSONObject data\)** — logs data for a run, equivalent to [wand.log\(\)](https://docs.wandb.com/library/log) at a specific step·   

    **run.finish\(int exitCode\)** — finishes a run with an exit code \(_default: 0_\) · 

     **run.log\(JSONObject data\)**—実行のデータをログに記録します。[wand.log\(\)](https://docs.wandb.com/library/log)と同等です。

     **run.log\(int step, JSONObject data\)**—実行のデータをログに記録します。これは、特定のステップでの[wand.log\(\)](https://docs.wandb.com/library/log)と同等です。·     

 **run.finish\(int exitCode\)**—終了コードで実行を終了します（デフォルト：0）

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







