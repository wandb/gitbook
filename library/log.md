---
description: '메트릭, 비디오, 사용자 지정 플롯 등 여러 자료를 추적하세요'
---

# wandb.log\(\)

 `wandb.log(dict)`을 호출해서 메트릭 사전 및 사용자 지정 객체를 한 단계에 로그하세요. 기본값으로, 저희는 매 회마다 단계를 점진적으로 증가시키므로, 시간이 지남에 따라 그래프와 풍부한 시각화로 모델의 출력을 확인하실 수 있습니다.  


**키워드 전달인자:**

* **Step — 로그를 연결하는 시간 단계 \( 확인\)**
* **Commit — 기본값은 commit=true으로 wandb.log를 호출할 때마다, 점진적으로 단계를 증가시킵니다. 여러 순차 wandb.log\(\) 명령을 동일한 단계에 저장하시려면 commit=false을 설정하세요.**

  
  **사용 예시:**

```python
wandb.log({'accuracy': 0.9, 'epoch': 5})
```

##  **객체 로깅**

저희는 이미지, 비디오, 오디오, 사용자 정의 그래프 등을 지원합니다. 풍부한 미디어를 로깅하셔서 결과값을 탐색하고 실행 간 비교를 시각화하실 수 있습니다.

 ​[Colab에서 해보기](https://colab.research.google.com/drive/15MJ9nLDIXRvy_lCwAou6C2XN3nppIeEK)​

###  **히스토그램**

```python
wandb.log({"gradients": wandb.Histogram(numpy_array_or_sequence)})
wandb.run.summary.update({"gradients": wandb.Histogram(np_histogram=np.histogram(data))})
```

시퀀스가 첫 번째 전달인자로 제공되는 경우, 저희는 자동으로 히스토그램을 비닝\(binning\)합니다. 또한, 여러분의 비닝\(binning\)을 하기 위해 np.histogram 으로 부터 반환된 내용을 np\_histogram 키워드 전달인자로 전달하여, 비닝\(binning\)을 할 수도 있습니다. 지원되는 빈\(bin\)의 최대 개수는 512개입니다. 64개의 빈의 기본값을 오버라이딩\(override\)하기 위해, 선택적 num\_bins 키워드 전달인자를 사용하실 수 있습니다.

만약 히스토그램이 요약\(summary\)에 있으면, 개별 실행 페이지의 히스토리에 스파크라인\(sparklines\)이 나타난다면, 시간이 지남에 따라 저희는 빈\(bins\)의 히트맵\(heatmap\)을 작성합니다.

###  **이미지 및 오버레이**

{% tabs %}
{% tab title="이미지" %}
`wandb.log({"examples": [wandb.Image(numpy_array_or_pil, caption="Label")]})`

 넘파이 배열\(numpy array\)가 제공된 경우, 저희는 마지막 차원\(dimension\)이 1이면, 그레이 스케일, 3인 경우 RGB, 4이면 RGBA라고 가정합니다. 만약 배열이 플롯\(float\)을 포함하고 있는 경우, 0에서 255사이의 정수\(ints\)로 변환합니다. 수동으로 [모드\(mode\)](https://pillow.readthedocs.io/en/3.1.x/handbook/concepts.html#concept-modes)를 지정하거나 또는 `PIL.Image`만 제공할 수 있습니다. 단계당 50개 미만의 이미지를 로그하실 것을 추천합니다
{% endtab %}

{% tab title="분할 마스크\(Segmentation Mask\)" %}
의미 분할\(sematic segmentation\)을 위한 마스크가 있는 이미지가 있는 경우, 마스크를 로그\(log\)하고 이를 토글 온/오프\(toggle on/off\) 하실 수 있습니다. 여러 마스크를 로그 하시려면, 마스크 사전을 여러 키\(key\)와 로그 하십시오. 여기 몇 가지 예시가 있습니다.

* **mask\_data**: 각 픽셀에 대한 정수 클래스 라벨\(integer class label\)을 포함한 2D 넘파이 배열
* **class\_labels**: **mask\_data**에서의 숫자를 읽을 수 있는 라벨로 매핑하는 사전

```python
mask_data = np.array([[1, 2, 2, ... , 2, 2, 1], ...])

class_labels = {
  1: "tree",
  2: "car",
  3: "road"
}

mask_img = wandb.Image(image, masks={
  "predictions": {
    "mask_data": mask_data,
    "class_labels": class_labels
  },
  "groud_truth": {
    ...
  },
  ...
})
```

 [라이브 예시 보기 →](https://app.wandb.ai/stacey/deep-drive/reports/Image-Masks-for-Semantic-Segmentation--Vmlldzo4MTUwMw)

 [샘플 코드 →](https://colab.research.google.com/drive/1SOVl3EvW82Q4QKJXX6JtHye4wFix_P4J)  


![](../.gitbook/assets/semantic-segmentation.gif)
{% endtab %}

{% tab title="바운딩 박스\(Bounding Box\)" %}
 이미지와 함께 바운딩 박스를 로그하고 필터와 토그를 사용해서 UI의 여러 박스 집합을 동적으로 시각화합니다.

```python
class_id_to_label = {
    1: "car",
    2: "road",
    3: "building",
    ....
}

img = wandb.Image(image, boxes={
    "predictions": {
        "box_data": [{
            "position": {
                "minX": 0.1,
                "maxX": 0.2,
                "minY": 0.3,
                "maxY": 0.4,
            },
            "class_id" : 2,
            "box_caption": "minMax(pixel)",
            "scores" : {
                "acc": 0.1,
                "loss": 1.2
            },
        }, 
        # Log as many boxes as needed
        ...
        ],
        "class_labels": class_id_to_label
    },
    "ground_truth": {
    # Log each group of boxes with a unique key name
    ...
    }
})

wandb.log({"driving_scene": img})
```

 선택적 매개변수

 `class_labels` class\_ids를 으로 스트링 값으로 매핑하는 선택적 전달인자. 기본값으로 저희는 `class_labels` `class_0`, `class_1`, 등을 생성합니다.

 Boxes – box\_data로 전달되는 각 각의 박스는 여러 좌표계\(coordinate systems\)와 함께 정의될 수 있습니다.

`position`

* 옵션 1: `{minX, maxX, minY, maxY}` 각 박스의 차원의 상한과 하한을 정의하는 좌표 세트를 제공합니다.
* 옵션 2: `{middle, width, height}` 중간 좌표를 \[x,y\]로 지정하고 `width`, `height`를 스칼라\(scalars\)로 지정하는 좌표 세트를 제공합니다.

`domain` 데이터 표현에 따라 위치 값의 도메인을 번경합니다  


* `percentage` \(기본값\(Default\)\) 이미지의 퍼센트를 거리로 나타내는 상대 값
* `pixel` 절대 픽셀 값

 [라이브 예시 보기 →](https://app.wandb.ai/stacey/yolo-drive/reports/Bounding-Boxes-for-Object-Detection--Vmlldzo4Nzg4MQ)​

![](../.gitbook/assets/bb-docs.jpeg)
{% endtab %}
{% endtabs %}

###  **미디어**

{% tabs %}
{% tab title="오디오" %}
```python
wandb.log({"examples": [wandb.Audio(numpy_array, caption="Nice", sample_rate=32)]})
```

 단개별로 로그할 수 있는 오디오 클립의 최대 개수는 100개입니다.
{% endtab %}

{% tab title="비디오" %}
```python
wandb.log({"video": wandb.Video(numpy_array_or_path_to_video, fps=4, format="gif")})
```

 넘파이 배열\(numpy array\)가 제공되는 경우, 차원은 :time\(시간\), channels\(채널\), width\(폭\), height\(높이\)로 가정합니다. 기본값으로 4 fps gif 이미지를 생성합니다 \(넘파이 객체\(numpy objects\)를 전달할 때, ffmpeg 및 moviepy python 라이브러리가 요구됩니다\). 지원 포맷은 "gif", "mp4", "webm", "ogg"입니다. 스트링을 `wandb.Video` 로 전달하는 경우, 저희는 wandb에 업로드하기 전 파일이 존재하고 지원되는 포맷인지 확인합니다. BytesIO 객체를 전달하면 지원되는 형식을 확장자로 사용하는 임시 파일을 생성합니다.

W&B실행 페이지의 미디어 섹션에서 비디오를 확인하실 수 있습니다.
{% endtab %}

{% tab title="텍스트 테이블" %}
Use wandb.Table\(\) to log text in tables to show up in the UI. By default, the column headers are UI에 표시할 테이블에 텍스트를 로그하시려면 wandb.Table\(\)를 사용하세요. 기본값으로 열 머리글\(column header\)는 `["Input", "Output"`, "Expected"\]입니다. 최대 행의 수는 10,000입니다.

```python
# Method 1
data = [["I love my phone", "1", "1"],["My phone sucks", "0", "-1"]]
wandb.log({"examples": wandb.Table(data=data, columns=["Text", "Predicted Label", "True Label"])})

# Method 2
table = wandb.Table(columns=["Text", "Predicted Label", "True Label"])
table.add_data("I love my phone", "1", "1")
table.add_data("My phone sucks", "0", "-1")
wandb.log({"examples": table})
```

```python
table = wandb.Table(dataframe=my_dataframe)
```
{% endtab %}

{% tab title="HTML" %}
```python
wandb.log({"custom_file": wandb.Html(open("some.html"))})
wandb.log({"custom_string": wandb.Html('<a href="https://mysite">Link</a>')})
```

 사용자 지정 html은 임의이 키에 로그될 수 있으며, 이는 실행페이지의 HTML 패널에 노출됩니다. 기본값으로 저희는 기본값 스타일\(default style\)을 투입하며, 기본 스타일은 `inject=False`을 전달함으로써 비활성화 하실 수 있습니다.

```python
wandb.log({"custom_file": wandb.Html(open("some.html"), inject=False)})
```
{% endtab %}

{% tab title="Molecule" %}
```python
wandb.log({"protein": wandb.Molecule(open("6lu7.pdb"))}
```

 10가지 파일 형식 중 하나로 분자 데이터\(molecular data\)를 로그합니다.

`'pdb', 'pqr', 'mmcif', 'mcif', 'cif', 'sdf', 'sd', 'gro', 'mol2', 'mmtf'`

 실행이 완료되면, UI에서 분자\(molecules\) 의 3D 시각화와 상호작용 할 수 있습니다.

 [라이브 예시 보기 →](https://app.wandb.ai/nbaryd/Corona-Virus/reports/Visualizing-Molecular-Structure-with-Weights-%26-Biases--Vmlldzo2ODA0Mw)​

![](../.gitbook/assets/docs-molecule.png)
{% endtab %}
{% endtabs %}

###  **사용자정의 차트**

 이러한 프리셋\(presets\)에는 스크립트에서 직접 차트를 로깅\(log\)하고 UI에서 찾고 있는 정확한 시각화를 확인할 수 있는 `wandb.plot` 수단이 내장되어 있습니다.

{% tabs %}
{% tab title="라인 플롯\(Line plot\)" %}
`wandb.plot.line()`

임의의 축x와 y에 연결되고 정렬된 점\(x,y\)의 목록인 사용자 지정 라인 플롯\(line plot\)을 로그합니다.

```python
data = [[x, y] for (x, y) in zip(x_values, y_values)]
table = wandb.Table(data=data, columns = ["x", "y"])
wandb.log({"my_custom_plot_id" : wandb.plot.line(table, "x", "y", title="Custom Y vs X Line Plot")})
```

이 옵션을 두 차원 상의 로그 곡선에 이용할 수 있습니다. 각 각에 대한 두 개의 값의 리스트를 비교하여 나타내는 경우, 리스트의 값의 수는 정확하게 일치해야 합니다. \(즉, 각 점에는 반드시 x와 y가 있어야 함\)

 [앱에서 보기 →](https://wandb.ai/wandb/plots/reports/Custom-Line-Plots--VmlldzoyNjk5NTA)​

 ​[코드 실행하기 →](https://tiny.cc/custom-charts)​
{% endtab %}

{% tab title="Scatter plot" %}
`wandb.plot.scatter()`

 임의의 축 x와 y의 쌍의 점 \(x, y\)의 목록인 사용자 정의 산점도\(scatter plot\)을 로그합니다.

```python
data = [[x, y] for (x, y) in zip(class_x_prediction_scores, class_y_prediction_scores)]
table = wandb.Table(data=data, columns = ["class_x", "class_y"])
wandb.log({"my_custom_id" : wandb.plot.scatter(table, "class_x", "class_y")})
```

 이를 사용해서 어느 두 차원에 산점도를 로그할 수 있습니다. 각 각에 대한 두 개의 값의 리스트를 비교하여 나타내는 경우, 리스트의 값의 수는 정확하게 일치해야 합니다. \(즉, 각 점에는 반드시 x와 y가 있어야 함\)

![](../.gitbook/assets/demo-scatter-plot.png)

[See in the app →](https://wandb.ai/wandb/plots/reports/Custom-Scatter-Plots--VmlldzoyNjk5NDQ)

[Run the code →](https://tiny.cc/custom-charts)
{% endtab %}

{% tab title="바 차트\(Bar chart\)" %}
`wandb.plot.bar()`

막대로 분류된 값의 리스트인 바 차트를 몇 개의 라인으로 로그합니다.

```python
data = [[label, val] for (label, val) in zip(labels, values)]
table = wandb.Table(data=data, columns = ["label", "value"])
wandb.log({"my_bar_chart_id" : wandb.plot.bar(table, "label", "value", title="Custom Bar Chart")
```

이를 사용해서 임의의 바 차트를 로그할 수 있습니다. 목록의 라벨 및 값의 수는 반드시 일치해야 합니다 \(즉, 각 각의 데이터 점에는 둘 다 있어야 함\).

![](../.gitbook/assets/image%20%2896%29.png)

 [앱에서 보기 →](https://wandb.ai/wandb/plots/reports/Custom-Bar-Charts--VmlldzoyNzExNzk)

 [코드 실행하기 →](https://tiny.cc/custom-charts)
{% endtab %}

{% tab title="Histogram" %}
`wandb.plot.histogram()`

Log a custom histogram—sort list of values into bins by count/frequency of occurrence—natively in a few lines. Let's say I have a list of prediction confidence scores \(`scores`\) and want to visualize their distribution:

```python
data = [[s] for s in scores]
table = wandb.Table(data=data, columns=["scores"])
wandb.log({'my_histogram': wandb.plot.histogram(table, "scores", title=None)})
```

You can use this to log arbitrary histograms. Note that `data` is a list of lists, intended to support a 2D array of rows and columns.

![](../.gitbook/assets/demo-custom-chart-histogram.png)

[See in the app →](https://wandb.ai/wandb/plots/reports/Custom-Histograms--VmlldzoyNzE0NzM)

[Run the code →](https://tiny.cc/custom-charts)
{% endtab %}

{% tab title="PR curve" %}
`wandb.plot.pr_curve()`

Log a [Precision-Recall curve](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_recall_curve.html#sklearn.metrics.precision_recall_curve) in one line:

```python
wandb.log({"pr" : wandb.plot.pr_curve(ground_truth, predictions,
                     labels=None, classes_to_plot=None)})
```

You can log this whenever your code has access to:

* a model's predicted scores \(`predictions`\) on a set of examples
* the corresponding ground truth labels \(`ground_truth`\) for those examples
* \(optionally\) a list of the labels/class names \(`labels=["cat", "dog", "bird"...]` if label index 0 means cat, 1 = dog, 2 = bird, etc.\)
* \(optionally\) a subset \(still in list format\) of the labels to visualize in the plot

![](../.gitbook/assets/demo-precision-recall.png)

[See in the app →](https://wandb.ai/wandb/plots/reports/Plot-Precision-Recall-Curves--VmlldzoyNjk1ODY)

[Run the code →](https://colab.research.google.com/drive/1mS8ogA3LcZWOXchfJoMrboW3opY1A8BY?usp=sharing)
{% endtab %}

{% tab title="ROC curve" %}
`wandb.plot.roc_curve()`

Log an [ROC curve](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_curve.html#sklearn.metrics.roc_curve) in one line:

```text
wandb.log({"roc" : wandb.plot.roc_curve( ground_truth, predictions, \
                        labels=None, classes_to_plot=None)})
```

You can log this whenever your code has access to:

* a model's predicted scores \(`predictions`\) on a set of examples
* the corresponding ground truth labels \(`ground_truth`\) for those examples
* \(optionally\) a list of the labels/ class names \(`labels=["cat", "dog", "bird"...]` if label index 0 means cat, 1 = dog, 2 = bird, etc.\)
* \(optionally\) a subset \(still in list format\) of these labels to visualize on the plot

![](../.gitbook/assets/demo-custom-chart-roc-curve.png)

[See in the app →](https://wandb.ai/wandb/plots/reports/Plot-ROC-Curves--VmlldzoyNjk3MDE)

[Run the code →](https://colab.research.google.com/drive/1_RMppCqsA8XInV_jhJz32NCZG6Z5t1RO?usp=sharing)
{% endtab %}
{% endtabs %}

### 사용자 지정 프리셋\(Custom presets\)

 내장 사용자 지정 차트 프리셋 변경 또는 새로운 프리셋을 생성하여, 차트를 저장합니다. 차트 ID를 사용하여 스크립트에서 직접 사용자 프리셋에 데이터를 로그합니다.

```python
# Create a table with the columns to plot
table = wandb.Table(data=data, columns=["step", "height"])

# Map from the table's columns to the chart's fields
fields = {"x": "step",
          "value": "height"}

# Use the table to populate the new custom chart preset
# To use your own saved chart preset, change the vega_spec_name
my_custom_chart = wandb.plot_table(vega_spec_name="carey/new_chart",
              data_table=table,
              fields=fields,
              )
```

[Run the code →](https://tiny.cc/custom-charts)

### Matplotlib

```python
import matplotlib.pyplot as plt
plt.plot([1, 2, 3, 4])
plt.ylabel('some interesting numbers')
wandb.log({"chart": plt})
```

 `matplotlib` 파이플롯\(pyplot\) 또는 figure object를 `wandb.log()`에 전달할 수 있습니다. 기본값으로 플롯을 [플로틀리\(Plotly\)](https://plot.ly/) 플롯으로 변환 해 드립니다. 플롯을 분명하게 이미지로 로그하고 싶으시다면, 플롯을 `wandb.Image`로 전달하실 수 있습니다. 또한 플로틀리\(Plotly\)차트를 직접적으로 로그하실 수 있습니다.  


###  **3D 시각화**

{% tabs %}
{% tab title="3D 객체" %}
`obj`, `gltf`, `glb` 포맷의 파일을 로그하고 실행이 완료되면 UI에서 해당 파일을 렌더링 해드립니다.

```python
wandb.log({"generated_samples":
           [wandb.Object3D(open("sample.obj")),
            wandb.Object3D(open("sample.gltf")),
            wandb.Object3D(open("sample.glb"))]})
```

![&#xD5E4;&#xB4DC;&#xD3F0; &#xD3EC;&#xC778;&#xD2B8; &#xD074;&#xB77C;&#xC6B0;&#xB4DC;&#xC758; &#xCC38; &#xAC12;\(Ground Truth\) &#xBC0F; &#xC608;&#xCE21;](../.gitbook/assets/ground-truth-prediction-of-3d-point-clouds.png)
{% endtab %}

{% tab title="포인트 클라우드" %}
3D 포인트 클라우드와 Lidar 장면\(scene\) 바운딩 박스와 함께 로그합니다. 렌더링할 점에 대한 좌표와 색상을 포함한 넘파이 배열에 전달합니다.

```python
point_cloud = np.array([[0, 0, 0, COLOR...], ...])

wandb.log({"point_cloud": wandb.Object3D(point_cloud)})
```

유연한 색 배합\(color schemes\)에 대한 세 가지 형태의 넘파이 배열이 지원됩니다.

* `[[x, y, z], ...]` `nx3`
* `[[x, y, z, c], ...]` `nx4` `| c is a category` 는 범위 `[1, 14]`의 a category 입니다. \(분할 시 유용\)
* `[[x, y, z, r, g, b], ...]` `nx6 | r,g,b` 는 빨강, 초록, 파랑 색상 채널에 대한 범위 `[0,255]` 내의 값입니다.

 아래는 로깅코드의 예시힙니다:

* `points`는 위에 표시된 심플한 포인트 클라우드 렌더러\(point cloud renderer\)와 동일한 포맷의 넘파이 배열입니다.
* `boxes` 는 다음 세 가지 속성을 가진 python 사전의 넘파이 배열입니다
  * `corners`- 8개의 코너의 리스트
  * `label`- 박스에서 렌더링될 라벨을 나타내는 스트링 \(선택적\)
  * `color`- 박스의 색상을 나타내는 rgb 값
* `type`  은 렌더링 할 장면\(scene\) 유형을 나타내는 스트링입니다. 현재 유일하게 지원되는 값은 `lidar/beta` 입니다.

```python
# Log points and boxes in W&B
wandb.log(
        {
            "point_scene": wandb.Object3D(
                {
                    "type": "lidar/beta",
                    "points": np.array(
                        [
                            [0.4, 1, 1.3], 
                            [1, 1, 1], 
                            [1.2, 1, 1.2]
                        ]
                    ),
                    "boxes": np.array(
                        [
                            {
                                "corners": [
                                    [0,0,0],
                                    [0,1,0],
                                    [0,0,1],
                                    [1,0,0],
                                    [1,1,0],
                                    [0,1,1],
                                    [1,0,1],
                                    [1,1,1]
                                ],
                                "label": "Box",
                                "color": [123,321,111],
                            },
                            {
                                "corners": [
                                    [0,0,0],
                                    [0,2,0],
                                    [0,0,2],
                                    [2,0,0],
                                    [2,2,0],
                                    [0,2,2],
                                    [2,0,2],
                                    [2,2,2]
                                ],
                                "label": "Box-2",
                                "color": [111,321,0],
                            }
                        ]
                    ),
                    "vectors": [
                        {"start": [0,0,0], "end": [0.1,0.2,0.5]}
                    ]
                }
            )
        }
    )
```
{% endtab %}
{% endtabs %}

## **점진적 로깅** 

 코드의 여러 다른 위치에서 단일 히스토리 단계로 로그하고 싶다면, 다음과 같이 스텝 인덱스\(step index\)를 `wandb.log()`로 전달할 수 있습니다.

```python
wandb.log({'loss': 0.2}, step=step)
```

단계별 동일한 값을 계속 전달하기만 하신다면, W&B는 하나의 통합된 사전에 각 각의 요청으로부터 키\(keys\)와 값을 수집합니다. 이전과 다른`step` 에 대한 값으로 `wandb.log()`를 요청하는 즉시 W&B는 모든 수집된 키와 값을 히스토리에 기록하고, 다시 수집을 시작합니다. 이는, “step: 0, 1, 2, ...에 대한 연속적인 값만을 사용하셔야 함을 의미을 유의하시기 바랍니다. 이 기능 절대적으로 어떠한 히스토리 단계에 기록하도록 허용하지 않으며, 오직 “current\(현재\)” 및 “next\(다음\)” 단계에만 작성하실 수 있습니다.

또한, 메트릭을 모으기 위해 `wandb.log`에 **commit=False**을 설정하실 수 있으며, 메트릭을 지속하기 위해 **commit\(커밋\) 플래그 없이   `wandb.log`**을 요청 하셔야 함을 유의하시기 바랍니다.

```python
wandb.log({'loss': 0.2}, commit=False)
# Somewhere else when I'm ready to report this step:
wandb.log({'accuracy': 0.8})
```

##  **요약 메트릭\(Summary Metrics\)**

요약 통계는 모델 당 단일 메트릭을 추적하는 데 사용됩니다. 요약 메트릭이 수정된 경우, 오직 업데이트된 상태만 저장됩니다. 여러분께서 수동으로 수정하신 경우가 아니라면 자동으로 추가된 마지막 히스토리 행으로 요약을 설정합니다.

```python
wandb.init(config=args)

best_accuracy = 0
for epoch in range(1, args.epochs + 1):
  test_loss, test_accuracy = test()
  if (test_accuracy > best_accuracy):
    wandb.run.summary["best_accuracy"] = test_accuracy
    best_accuracy = test_accuracy
```

훈련이 완료된 후 평가 메트릭을 실행 요약에 저장하시는 것이 좋습니다. 요약은 넘파이 배열, pytorch tensors, tensorflow tensors를 처리할 수 있습니다. 값이 이러한 유형 중 하나일 때, 저희는 전체 tensor를 이진 파일\(binary file\)에 유지하고, min\(최소값\). mean\(평균\), variance\(분산\), 95% percentile\(95% 백분위\)와 같은 최대 수준의 메트릭을 요약 객체에 저장합니다.

```python
api = wandb.Api()
run = api.run("username/project/run_id")
run.summary["tensor"] = np.random.random(1000)
run.summary.update()
```

###  **로그에 직접 액세스하기**

히스토리 객체는 wandb.log에 의해 로그된 메트릭을 추적하는 데 사용됩니다. `run.history.row`를 통해서 메트릭의 가변사전\(mutable dictionary\)에 액세스하실 수 있습니다. 행은 저장되고 run.history.add 또는 `wandb.log`이 요청될 때 생성된 새 행이 생성됩니다.

####  **Tensorflow 얘시**

```python
wandb.init(config=flags.FLAGS)

# Start tensorflow training
with tf.Session() as sess:
  sess.run(init)

  for step in range(1, run.config.num_steps+1):
      batch_x, batch_y = mnist.train.next_batch(run.config.batch_size)
      # Run optimization op (backprop)
      sess.run(train_op, feed_dict={X: batch_x, Y: batch_y})
      # Calculate batch loss and accuracy
      loss, acc = sess.run([loss_op, accuracy], feed_dict={X: batch_x, Y: batch_y})

      wandb.log({'acc': acc, 'loss':loss}) # log accuracy and loss
```

#### **PyTorch 예시**

```python
# Start pytorch training
wandb.init(config=args)

for epoch in range(1, args.epochs + 1):
  train_loss = train(epoch)
  test_loss, test_accuracy = test()

  torch.save(model.state_dict(), 'model')

  wandb.log({"loss": train_loss, "val_loss": test_loss})
```

##  **공통된 질문들**

###  **다른 에포크\(epoch\)에서 이미지 비교하기**

 단계에서 이미지를 로그 할 때마다, UI에 나타나도록 이미지를 저장합니다. 이미지 패널을 고정하고, step slider를 사용하여 다른 단계의 이미지를 확인합니다. 이를 통해 모델의 출력이 훈련에 따라 어떻게 변하는지 쉽게 비교할 수 있습니다.

```python
wandb.log({'epoch': epoch, 'val_acc': 0.94})
```

###  **배치 로깅**

모든 배치에 특정 메트릭을 로그하고 플롯을 표준화 하시려면, 메트릭과 함께 작성 하고 싶은 x축 값을 로그하실 수 있습니다. 그 후, 사용자 정의 플롯에서, edit를 클릭하고, custom x-axis\(사용자 정의 x축\)을 선택합니다.

```python
wandb.log({'batch': 1, 'loss': 0.3})
```

###  **PNG 로깅**

wandb.Image는 기본값으로 넘파이 배열 또는 PILImage의 인스턴스를 PNG로 변환합니다.

```python
wandb.log({"example": wandb.Image(...)})
# Or multiple images
wandb.log({"example": [wandb.Image(...) for img in images]})
```

### **JPEG 로깅**

 JPEG를 저장하시려면 경로를 파일에 전달하시면 됩니다:

```python
im = PIL.fromarray(...)
rgb_im = im.convert('RGB')
rgb_im.save('myimage.jpg')
wandb.log({"example": wandb.Image("myimage.jpg")})
```

###  **비디오 로깅**

```python
wandb.log({"example": wandb.Video("myvideo.mp4")})
```

 이제 미디어 브라우저에서 동영상을 볼 수 있습니다. 프로젝트 작업영역으로 가서, 작업영역을 실행하시거나 리포트 및 “Add visualization\(시각화 추가하기\)”를 클릭하여 다채로운 미디어 패널을 추가합니다.

###  **사용자 정의 x-축**

 기본값으로, `wandb.log` 요청하실 때 마다 저희는 점차적으로 글로벌 단계를 증가합니다. 원하시는 경우, 단조적으로 증가하는 단계를 로그하고 그래프에서 사용자 정의 x-축으로 선택하실 수 있습니다.

 예를 들어, 정렬하고 싶은 훈련 및 검증 단계에 있는 경우, 여러분의 step counter: `wandb.log({“acc”:1, “global_step”:1})`.

를 저희에게 전달 해 주십시오. 그리고 나서, 그래프에서 “global\_step”을 x-축으로 선택 해주십시오.`wandb.log({“acc”:1,”batch”:10}, step=epoch)`를 사용하시면 기본값 단계 축 외에도 “batch”를 x축으로 선택 하실 수 있습니다.

###  **포인트 클라우드 탐색 및 줌**

컨트롤\(Ctrl\)을 누르시고 마우스를 사용해서 공간 내에서 이동하실 수 있습니다.

###  **그래프에 아무것도 표시되지 않습니다** 

 “No visualization data logged yet\(아직 로그된 시각화 데이터가 없음\)”이 나타난다면, 스크립트에서 아직 첫 번째 wandb.log 요청을 받지 못했음을 의미합니다. 이는 실행 완료까지 오랜 시간이 걸리기 때문 일수도 있습니다. 각 에포크\(epoch\)의 끝에 로그 하는 경우, 데이터 스트림을 보다 빠르게 보기 위해 에포크 별로 몇 번 로깅 하실 수 있습니다.

###  **메트릭 이름 복제**

 같은 키로 다른 유형을 로그하는 경우, 저희는 데이터베이스에서 이것들을 분리해야만 합니다. 즉, UI의 드롭다운 메뉴에서 같은 메트릭 이름의 여러 항목이 나타남을 의미합니다. 저희가 그룹화하는 기준은 숫자, 스트링, bool, 기타 \(주로 배열\(arrays\)\), 모든 wandb 유형 \(히스토그램, 이미지 등\)입니다. 이러한 동작\(behavior\)을 피하시려면 각 키에 하나의 유형만 전송하시기 바랍니다.

###  **퍼포먼스 및 제한**

 **샘플링**

 포인트를 더 많이 보낼수록, UI에서 그래프를 로딩하는 시간이 더 길어집니다. 라인에 1000개 이상의 포인트가 있는 경우, 저희는 여러분의 브라우저로 데이터를 전송하기 전에 백엔드에서 1000개의 포인트로 줄여서 샘플링합니다. 이 샘플링은 비결정적\(nondeterministic\)이므로, 페이지를 새로 고침 하시면 다른 세트의 샘플링된 포인트를 확인하실 수 있습니다.

 원본 데이터 전부를 원하신다면, [data API](https://docs.wandb.ai/library/public-api-guide)를 사용하셔서 샘플링 되지 않은 데이터를 끌어 오실 수 있습니다.

 **가이드라인**

메트릭당 10,000 포인트 이하를 로그하시는 것을 추천 드립니다. 구성\(config\) 및 요약 메트릭 열\(column\)이 500개 이상인 경우, 저희는 테이블에 500개만 표시합니다. 100만개 이상의 포인트가 라인에 로그 된 경우, 페이지를 로딩하는 데 시간이 걸릴 수 있습니다. ****

 저희는 메트릭을 케이스 인센시티브 \(case-insensitive, 대/소문자를 구분하지 않는\) 방식으로 저장하므로 “My-Metric”과 “my-metric”과 같은 동일한 이름의 두 개의 메트릭을 가지고 있으실 수 없음을 유의하시기 바랍니다.

### **이미지 업로딩 제어**

 “제 프로젝트에 W&B를 통합하고 싶어요. 하지만 이미지는 업로드하고 싶지 않습니다.”

저희의 통합은 자동으로 이미지를 업로드하지 않습니다. 즉, 여러분이 명시적으로 업로드할 파일을 지정하셔야 합니다. 다음은 저희가 명시적으로 이미지: [http://bit.ly/pytorch-mnist-colab](http://bit.ly/pytorch-mnist-colab)를 로그한 PyTorch에 대한 간단한 예시입니다.

```python
wandb.log({
        "Examples": example_images,
        "Test Accuracy": 100. * correct / len(test_loader.dataset),
        "Test Loss": test_loss})
```

