# Log

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_run.py#L810-L958)[GitHub에서 소스 확인하기](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_run.py#L810-L958)​

글로벌 실행 히스토리에 dict를 로그합니다.

```text
log(
    data: Dict[str, Any],
    step: int = None,
    commit: bool = None,
    sync: bool = None
) -> None
```

스칼라에서 히스토그램, 미디어, matplotlib 플롯에 이르기까지 모든 것을 로그하는 데 `wandb.log`를 사용할 수 있습니다.

가장 기본적인 사용법은 `wandb.log({'train-loss': 0.5, 'accuracy': 0.9})`입니다. 이를 통해 train-loss=0.5 및 `accuracy=0.9`인 실행과 관련된 히스토리 행을 저장합니다. 히스토리 값은 app.wadb.ai 또는 로컬 서버에서 플로팅할 수 있습니다. 또한 히스토리 값을 wandb API를 통해 다운로드할 수도 있습니다.

 값을 로깅하면 로그된 메트릭에 대한 요약 값이 업데이트됩니다. 요약 값은 app.wandb.ai의 실행 테이블 또는 로컬 서버에 나타납니다. 예를 덜어 요약 값을 수동으로 `wandb.run.summary["accuracy"] = 0.9`과 같이 설정하면, `wandb.log`는 더 이상 자동으로 실행의 정확성을 업데이트하지 않습니다.

 값을 로깅하는 것은 스칼라일 필요가 없습니다. 모든 wandb 객체 로깅이 지원됩니다. 예를 들어, `wandb.log({"example": wandb.Image("myimage.jpg")})`는 wandb UI에 깔끔하게 표시되는 예시 이미지를 로그합니다. 지원되는 모든 유형의 경우, [https://docs.wandb.com/library/reference/data\_types](https://docs.wandb.com/library/reference/data_types)를 참조하시기 바랍니다.

 중첩된\(nested\) 메트릭 로깅이 권장되며, wandb API에서 지원됩니다. 따라서 여러 정확도 값을 `wandb.log({'dataset-1': {'acc': 0.9, 'loss': 0.3} ,'dataset-2': {'acc': 0.8, 'loss': 0.2}})`과 함께 로그할 수 있으며, 메트릭은 wandb UI에서 구성됩니다.

W&B는 글로벌 단계\(global step\)를 추적하므로 관련된 메트릭을 함께 로깅하는 것을 권장합니다. 따라서 기본값으로 wandb.log가 호출될 때마다, 글로벌 단계는 증가됩니다. 관련 메트릭을 함께 호출하는 것이 불편한 경우, `wandb.log({'train-loss': 0.5, commit=False})`을 호출한 다음 `wandb.log({'accuracy': 0.9})` 호출하는 것은 `wandb.log({'train-loss': 0.5, 'accuracy': 0.9})`을 호출하는 것과 동등합니다.

wandb.log는 초당 몇 번 이상 호출되지 않습니다. 이보다 더 자주 로그하기를 원하신다면 클라이언트 측에서 데이터를 종합하는 것이 좋으며 그렇지 않은 경우 퍼포먼스가 저하될 수 있습니다.

| 전달인자 |
| :--- |
|  row \(dict, optional\): 직렬화할 수 있는 Python 객체의 dict로 즉, `str`, `ints`, `floats`, `Tensors`, `dicts`, `wandb.data_types`입니다. commit \(boolean, optional\): 메트릭 dict를 wandb 서버에 저장하여 단계를 증가시킵니다. false인 경우 `wandb.log`는 행 전달인자와 함께 현재 메트릭 dict만 업데이트하며 `wandb.log`가 `commit=True`와 함께 호출될 때까지 메트릭은 저장되지 않습니다. step \(integer, optional\): 프로세싱의 글로벌 단계입니다. 커밋되지 않은\(non-committed\) 이전 단계를 지속시키나, 기본값으로 지정된 단계를 커밋하지 않도록 되어있습니다. sync \(boolean, True\): 이 전달인자는 더 이상 사용할 수 없으며 현재 `wandb.log`의 동작\(behavior\)을 변경하지 않습니다. |

## **예시:**

기본 사용법

```python
wandb.log({'accuracy': 0.9, 'epoch': 5})
```

점진적 로깅\(Incremental logging\)

```python
wandb.log({'loss': 0.2}, commit=False)
# Somewhere else when I'm ready to report this step:
wandb.log({'accuracy': 0.8})
```

히스토그램

```python
wandb.log({"gradients": wandb.Histogram(numpy_array_or_sequence)})
```

이미지

```python
wandb.log({"examples": [wandb.Image(numpy_array_or_pil, caption="Label")]})
```

비디오

```python
wandb.log({"video": wandb.Video(numpy_array_or_video_path, fps=4,
    format="gif")})
```

Matplotlib 플롯

```python
wandb.log({"chart": plt})
```

PR 곡선

```python
wandb.log({'pr': wandb.plots.precision_recall(y_test, y_probas, labels)})
```

3D 개체

```python
wandb.log({"generated_samples":
[wandb.Object3D(open("sample.obj")),
    wandb.Object3D(open("sample.gltf")),
    wandb.Object3D(open("sample.glb"))]})
```

 더 자세한 예는 [https://docs.wandb.com/library/log](https://docs.wandb.com/library/log)를 참조하시기 바랍니다.

| 발생\(Raises\) |
| :--- |
| wandb.Error - `wandb.init` 이 전에 호출된 경우 ValueError – 잘못된 데이터가 전달된 경우 |

