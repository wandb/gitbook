---
description: Builderは、モデルメトリックをログに記録するトレーニングループの前に実行を構成および初期化するために使用されます。
---

# WandbRun.Builder

### 概要

Builderパターンを使用すると、読み取り可能なコードを記述してWandbRunをセットアップできます。ビルダーには、これらの値の初期化に役立ついくつかの関数が含まれています。

* **builder.build\(\)**—実行を表すWandbRunインスタンスを返します
* **builder.withName\(String name\)**—UIに表示され、編集可能なこの実行の表示名は、ユニークである必要はありません。
*   **builder.withConfig\(JSONObject data\)**—初期構成値を含むJava JSONオブジェクト
* **builder.withProject\(String project\)**—この実行が属するプロジェクトの名前
*    **builder.withNotes\(String notes\)**—実行に関連する説明
*    **builder.setTags\(List&lt;String&gt; tags\)**—実行で使用されるタグの配列
*  **builder.setJobType\(String type\)**—ログに記録するジョブのタイプ。例：eval、worker、ps（デフォルト：トレーニング）
* **builder.withGroup\(String group\)**—他の実行をグループ化するための文字列。グループ化を参照してください run

Most of these settings can also be controlled via [Environment Variables](../../library/environment-variables.md). This is often useful when you're running jobs on a cluster.   


これらの設定のほとんどは、環境変数を介して制御することもできます。これは、クラスターでジョブを実行しているときに役立つことがよくあります。

### 例

 デフォルト実行の初期化

```java
WandbRun run = new WandbRun.Builder().build();
```

 構成オブジェクトおよび名前を使用した実行の初期化

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

 



