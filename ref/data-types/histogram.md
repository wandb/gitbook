# Histogram

  
​[​![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L218-L283)[**GitHub에서 소스 확인하기**](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L218-L283)**​**​

히스토그램에 대한 wandb 클래스

```text
Histogram(    sequence=None, np_histogram=None, num_bins=64)
```

이 객체는 넘파이의 히스토그램 함수와 동일하게 작동합니다. [https://docs.scipy.org/doc/numpy/reference/generated/numpy.histogram.html](https://docs.scipy.org/doc/numpy/reference/generated/numpy.histogram.html)​

## **예시:** <a id="examples"></a>

시퀀스\(sequence\)에서 히스토그램을 생성합니다.

```text
wandb.Histogram([1,2,3])
```

np.histogram에서 효율적으로 초기화합니다.

```text
hist = np.histogram(data)wandb.Histogram(np_histogram=hist)
```

| **전달인자** | ​ |
| :--- | :--- |
|  `sequence` |   \(array\_like\) 히스토그램에 대한 입력 데이터 |
|  `np_histogram` |  \(numpy histogram\) 사전 계산된\(precoomputed\) 히스토그램의 대체 입력 |
|  `num_bins` |  \(int\) 히스토그램에 대한 빈\(bin\)의 수. 기본 빈\(bin\)의 수는 64입니다. 최대 빈의 수는 512입니다. |

| Attributes | ​ |
| :--- | :--- |
|  `bins` | \(\[float\]\) 빈\(bin\)의 가장자리\(edges\) |
|  `histogram` |  \(\[int\]\) 각 빈\(bin\)에 포함된 요소\(element\)의 수 |

| **클래스 변수** | ​ |
| :--- | :--- |
|  MAX\_LENGTH |  \`512\` |
|  artifact\_type |  \`None\` |

