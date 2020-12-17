---
description: wandb.data_types
---

# Data Types Reference

[소스](https://github.com/wandb/client/blob/master/wandb/data_types.py#L0)

Wandb에는 풍부한 시각화를 로그 하기 위한 특수 데이터 유형이 있습니다.

모든 특수 데이터 유형은 WBValue의 하위 클래스입니다. 모든 데이터 유형은 JSON으로 직렬화됩니다. 이는 wandb가 객체를 로컬에 저장하고 W&B 서버에 업로드하기 위해 사용하는 것이기 때문입니다.

## WBValue

  [소스](https://github.com/wandb/client/blob/master/wandb/data_types.py#L43)​

```python
WBValue(self)
```

 Wandb.log\(\)에 의해 로그 되고 wandb에 의해 시각화될 수 있는 항목에 대한 추상 부모 클래스\(abstract parent class\)

 객체는 JSON으로 직렬화되며 언제나 다른 필드를 해석하는 법을 나타내는 \_type 속성을 가집니다.

 **반환**:

추후에 스트링으로 직렬화될 수 있는 이 객체의 JSON 친화적 `dict` 표현

##  **히스토그램**

 [소스](https://github.com/wandb/client/blob/master/wandb/data_types.py#L64)​

```python
Histogram(self, sequence=None, np_histogram=None, num_bins=64)
```

 히스토그램에 대한 wandb 클래스

이 객체는 넘파이\(numpy\)의 히스토그램 함수처럼 작동합니다

[https://docs.scipy.org/doc/numpy/reference/generated/numpy.histogram.html](https://docs.scipy.org/doc/numpy/reference/generated/numpy.histogram.html)​

 **예시**:

 시퀀스에서 히스토그램을 생성합니다

```python
wandb.Histogram([1,2,3])
```

 np.histogram에서 효율적으로 초기화합니다.

```python
hist = np.histogram(data)
wandb.Histogram(np_histogram=hist)
```

 **전달인자**:

* `sequence` _array\_like_ - 히스토그램에 대한 입력 데이터
* `np_histogram` _numpy histogram_ - 미리 연산된 히스토그램의 대체 입력
* `num_bins` _int_ - 히스토그램에 대한 빈\(bins\)의 수. 빈의 기본 숫자는 64입니다. 최대 빈 개수는 512입니다.

 **속성**:

* `bins` _\[float\]_ -  빈의 에지\(edges\)
* `histogram` _\[int\]_ - 각 빈에 포함되는 요소의 수

##  **미디어**

 [소스](https://github.com/wandb/client/blob/master/wandb/data_types.py#L122)​

```python
Media(self, caption=None)
```

JSON의 외부에 파일로 저장하여 프론트 엔드의 미디어 패널에 표시되는 WBValue

필요 시, 저희는 파일을 실행의 미디어 디렉토리로 이동 또는 복사하여 업로드합니다.

## BatchableMedia

[소스](https://github.com/wandb/client/blob/master/wandb/data_types.py#L232)

```python
BatchableMedia(self, caption=None)
```

이미지 및 썸네일 처럼 배치로 특별히 취급하는 미디어의 부모 클래스

이미지 이 외에도, 저희는 이러한 배치를 사용하여 미디어 디렉토리의 파일을 이름 별로 정리합니다.

##  **테이블**

 [소스](https://github.com/wandb/client/blob/master/wandb/data_types.py#L244)

```python
Table(self,
      columns=['Input', 'Output', 'Expected'],
      data=None,
      rows=None,
      dataframe=None)
```

 테이블은 작은 기록\(records\) 세트를 표시하도록 설계된 테이블입니다.

 **전달인자**:

* `columns` _\[str\]_ - 테이블의 열의 이름. 기본값은 \["Input", "Output", "Expected"\]입니다.
* `data` _array_ - 스트링으로 표시될 값의 2D 배열
* `dataframe` _pandas.DataFrame_ - 테이블 생성에 사용되는 DataFrame 객체, 설정 시 다른 전달인자는 무시됩니다.

## **오디오**

 [소스](https://github.com/wandb/client/blob/master/wandb/data_types.py#L305)

```python
Audio(self, data_or_path, sample_rate=None, caption=None)
```

 오디오 클립에 대한 Wnadb 클래스

 **전달인자**:

* `data_or_path` _string or numpy array_ - 오디오 파일의 경로 또는 오디오 데이터의 넘파이 배열.
* `sample_rate` _int_ - 오디오 데이터의 원시\(raw\) 넘파이 배열을 전달될 때 요구되는 샘플 레이트\(sample rate\).
* `caption` _string_ - 오디오와 함께 표시할 캡션

## Object3D

[source](https://github.com/wandb/client/blob/master/wandb/data_types.py#L404)

```python
Object3D(self, data_or_path, **kwargs)
```

 3D 점구름\(point clouds\)에 대한 wandb 클래스

 **전달인자**:

data\_or\_path \(numpy array \| string \| io \): Object3D는 파일 또는 넘파이 배열에서 초기화할 수 있습니다.

지원되는 파일 형식은 obj, gltf, babylon, stl입니다. 파일 경로 또는 io 객체와 `'obj', 'gltf', 'babylon', '`stl'중 하나여야 하는 file\_type를 전달할 수 있습니다.

넘파이 배열의 형태는 다음 중 하나여야 합니다:

```python
[[x y z],       ...] nx3
[x y z c],     ...] nx4 where c is a category with supported range [1, 14]
[x y z r g b], ...] nx4 where is rgb is color
```

##  **분자\(Molecule\)**

 [소수](https://github.com/wandb/client/blob/master/wandb/data_types.py#L527)

```python
Molecule(self, data_or_path, **kwargs)
```

 분자 데이터에 대한 Wandb 클래스

 **전달인자**:

data\_or\_path \( string \| io \): 분자는 파일명 또는 io 객체에서 초기화할 수 있습니다.

## Html

  [소스](https://github.com/wandb/client/blob/master/wandb/data_types.py#L611)

```python
Html(self, data, inject=True)
```

임의의 html에 대한 Wandb 클래스

 **전달인자**:

* `data` _string or io object_ - wandb에 표시할 HTML
* `inject` _boolean_ - HTML 객체에 스타일시트를 추가합니다. False로 설정한 경우, HTML은 변경되지 않은 상태로 통과합니다.

##  **비디오**

[소스](https://github.com/wandb/client/blob/master/wandb/data_types.py#L680)

```python
Video(self, data_or_path, caption=None, fps=4, format=None)
```

Wandb 비디오 표현

**전달인자**:

data\_or\_path \(numpy array \| string \| io\): 비디오는 파일의 경로 또는 io 객체와 함께 초기화할 수 있습니다. 포맷은 반드시 "gif", "mp4", "webm" 또는 "ogg"여야 합니다. 포맷은 반드시 포맷 인자\(format argument\)와 함께 지정되어야 합니다. 비디오는 넘파이 tensor와 함께 비디오를 초기화할 수 있습니다. 넘파이 tensor는 4 또는 5 차원이어야 합니다. 채널은 \(time, channel, height, width\) 또는 \(batch, time, channel, height width\) 여야 합니다

* `caption` _string_ - 디스플레이하는 비디오와 연관된 캡션
* `fps` _int_ - 비디오의 초당 프레임. 기본값은 4입니다.
* `format` _string_ - 비디오의 포맷으로, 경로 또는 io 객체와 함께 초기활 할 경우 필요합니다.

##  **이미지**

 [소스](https://github.com/wandb/client/blob/master/wandb/data_types.py#L827)

```python
Image(self,
      data_or_path,
      mode=None,
      caption=None,
      grouping=None,
      boxes=None,
      masks=None)
```

이미지에 대한 Wandb 클래스

**전달인자**:

* `data_or_path` _numpy array \| string \| io_ - 이미지 데이터의 넘파이 배열 또는 PIL 이미지를 허용합니다. 클래스는 데이터 형식을 유추하여 변환합니다.
* `mode` _string_ -  이미지의 PIL 모드. 가장 일반적인 경우는 "L", "RGB", "RGBA"입니다. 전체 설명은 다음 페이지에서 확인하실 수 있습니다:[https://pillow.readthedocs.io/en/4.2.x/handbook/concepts.html\#concept-modes](https://pillow.readthedocs.io/en/4.2.x/handbook/concepts.html#concept-modes).
* `caption` _string_ - 이미지 표시를 위한 라벨

## **JSON메타데이터**

 [소스](https://github.com/wandb/client/blob/master/wandb/data_types.py#L1093)​

```python
JSONMetadata(self, val, **kwargs)
```

JSON메타데이터는 임의의 메타데이터를 파일로 인코딩하기 위한 유형입니다.

## BoundingBoxes2D

 [소스](https://github.com/wandb/client/blob/master/wandb/data_types.py#L1126)

```python
BoundingBoxes2D(self, val, key, **kwargs)
```

 2D 경계 상자에 대한 wandb 클래스

## ImageMask

 [소스](https://github.com/wandb/client/blob/master/wandb/data_types.py#L1204)

```python
ImageMask(self, val, key, **kwargs)
```

 이미지 마스크에 대한 Wandb 클래스로 분할 작업에 유용합니다.

## **플로틀리**

 [소스](https://github.com/wandb/client/blob/master/wandb/data_types.py#L1274)

```python
Plotly(self, val, **kwargs)
```

플로틀리 플롯에 대한 Wandb 클래스

**전달인자**:

* `val` - matplotlib 또는 플로틀리 figure

##  **그래프**

 [소스](https://github.com/wandb/client/blob/master/wandb/data_types.py#L1314)

```python
Graph(self, format='keras')
```

 그래프에 대한 Wandb 클래스

이 클래스는 일반적으로 신경망 모델의 저장 및 표시에 사용됩니다. 이는 그래프를 노드\(nodes\) 및 에지\(edges\)의 배열로 나타냅니다.

 **예시**:

keras 모델을 가져옵니다:

```python
Graph.from_keras(keras_model)
```

**속성**:

* `format` _string_ – wandb의 그래프 표시를 돕는 포맷
* `nodes` _\[wandb.Node\]_ - wandb.Nodes 리스트
* `nodes_by_id` _dict_ - dict of ids -&gt; nodes edges \(\[\(wandb.Node, wandb.Node\)\]\): 에지로 해석되는 노드 쌍의 리스트
* `loaded` _boolean_ - 그래프가 완전히 로드 되었는지 여부를 나타내는 플래그
* `root` _wandb.Node_ - 그래프의 루트 노드

## **노드**

 [소스](https://github.com/wandb/client/blob/master/wandb/data_types.py#L1470)

```python
Node(self,
     id=None,
     name=None,
     class_name=None,
     size=None,
     parameters=None,
     output_shape=None,
     is_output=None,
     num_parameters=None,
     node=None)
```

[그래프](https://docs.wandb.com/ref/data-types#graph)에 사용되는 노드

##  **에지**

 [소스](https://github.com/wandb/client/blob/master/wandb/data_types.py#L1636)

```python
Edge(self, from_node, to_node)
```

[그래프](https://docs.wandb.com/ref/data-types#graph)에 사용되는 에지

