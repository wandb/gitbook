---
description: Python에서 트리거 되어 Slack 또는 이메일로 전송되는 스크립트 가능 경보
---

# wandb.alert\(\)

Python 스크립트에서 트리거 되는 Slack 또는 이메일 경보를 전송합니다.

1.  [계정에 경보를 설정합니다 →](https://docs.wandb.ai/v/ko/app/features/alerts)
2.  [코드를 시험합니다 →](http://tiny.cc/wb-alerts)
3. Slack 또는 이메일을 체크하여 스크립트 가능 경보를 확인합니다.

### **전달인자**

`wandb.alert(title="Low Acc", text="Accuracy is below the expected threshold")`

* **title \(string\)**: 경보에 대한 간단한 설명. 예: “낮은 정확도”
* **text \(string\)**: 경보를 트리거한 작업에 대한 보다 길고 상세한 설명.
* **level \(optional\):**  경보의 중요도 — 반드시 `INFO`, `WARN`, 또는 `ERROR` 여야 합니다
* **wait\_duration \(optional\):** 같은 title\(제목\)의 다른 경고를 전송하기 전에 대기하는 시간\(초\). 이를 통해 경보 스팸을 줄일 수 있습니다.

###  **예시**

 다음의 단순한 경보는 정확도가 임계값\(threshold\) 아래로 떨어질 때 경고를 전송합니다. 스팸을 방지하기 위해 최소 5분 간격으로만 경보를 전송합니다.

 [코드 실행하기 →](http://tiny.cc/wb-alerts)

```python
from datetime import timedelta
import wandb
from wandb import AlertLevel

if acc < threshold:
    wandb.alert(
        title='Low accuracy', 
        text=f'Accuracy {acc} is below the acceptable theshold {threshold}',
        level=AlertLevel.WARN,
        wait_duration=timedelta(minutes=5)
    )
```

