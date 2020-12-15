---
description: WandbRun 객체는 Java에서 실행의 인스턴스를 나타냅니다.
---

# WandbRun

###  **개요**

[WandbRun Builder](https://docs.wandb.com/ref/java/wandbrun-builder)를 사용해서 실행을 생성할 수 있습니다. 이 객체는 실행을 추적하는데 사용됩니다

* **run.log\(JSONObject data\)** — 실행에 대한 데이터를 로그 합니다.[wand.log\(\)](https://docs.wandb.com/library/log)​와 동일
* **run.log\(int step, JSONObject data\)** —  실행에 대한 데이터를 로그 합니다. 특정 단계에서 [wand.log\(\)](https://docs.wandb.com/library/log)와 동일
* **run.finish\(int exitCode\)** — 종료 코드 \(기본값: 0\)를 통해 실행 종료

###  **예시**

Java 클라이언트를 통해 사인파\(sin wave\) 플로팅 하기

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







