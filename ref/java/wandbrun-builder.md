---
description: Builder는 모델 메트릭을 로그하는 훈련 루프 전에 실행을 구성하고 초기화하는 데 사용됩니다.
---

# WandbRun.Builder

###  **개요**

Builder 패턴을 사용하면 WandbRun 설정을 위한 판독 가능 코드\(readable code\)를 작성할 수 있습니다. Builder는 이러한 값을 초기화하는데 사용되는 몇 가지 함수를 포함하고 있습니다.

* **builder.build\(\)** — WandbRun 인스턴스를 반환하며, 실행을 나타냅니다.
* **builder.withName\(String name\)** — 이 실행에 대한 표시 이름으로, UI에 표시되며 편집할 수 있습니다. 고유할 필요가 없습니다.
* **builder.withConfig\(JSONObject data\)** — 기 구성 값을 포함하는 Java JSON 객체
* **builder.withProject\(String project\)** — 이 실행이 속할 프로젝트의 이름
* **builder.withNotes\(String notes\)** — 실행과 관련된 설명
* **builder.setTags\(List&lt;String&gt; tags\)** — 실행과 함께 사용되는 태그 배열
* **builder.setJobType\(String type\)** — 로그 중인 작업 유형. 예: eval, worker, ps \(기본값: training\)
* **builder.withGroup\(String group\)** — 다른 실행을 그룹화하는데 사용되는 스트링; [그룹화](https://docs.wandb.ai/v/ko/library/grouping) 참조

 이러한 설정의 대부분은 [환경 변수](https://docs.wandb.ai/v/ko/library/environment-variables)를 통해서도 제어할 수 있습니다. 클러스터\(cluster\)에서 작업을 실행 중일 때 유용합니다.

### **예시**

 기본값 실행 초기화하기

```java
WandbRun run = new WandbRun.Builder().build();
```

구성 객체 및 이름을 통해 실행 초기화하기

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

 



