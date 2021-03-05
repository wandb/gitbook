# Audio

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L862-L976)[GitHub에서 소스 확인하기](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L862-L976)**​**

오디오 클립에 대한 Wandb 클래스

```text
Audio(
    data_or_path, sample_rate=None, caption=None
)
```

| **전달인자** |  |
| :--- | :--- |
|  `data_or_path` |  \(string or numpy array\) 오디오 파일 경로 또는 오디오 데이터의 넘파이 배열. |
|  `sample_rate` |  \(int\) 샘플 레이트\(sample rate\)로 오디오 데이터의 원시\(raw\) 넘파이 배열을 전달할 때 필요합니다. |
|  `caption` | \(string\) 오디오와 함께 표시할 캡션 |

## **방법**

### `durations` <a id="durations"></a>

 **​**[**소스 보기**](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L952-L954)**​**

```text
@classmethod
durations(
    audio_list
)
```

### `sample_rates` <a id="sample_rates"></a>

 [**소스 보기**](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L956-L958)**​**

```text
@classmethod
sample_rates(
    audio_list
)
```

| **클래스 변수** |  |
| :--- | :--- |
|  artifact\_type |  \`'audio-file'\` |

