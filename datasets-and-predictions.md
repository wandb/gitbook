---
description: 데이터세트를 반복하고 모델 예측을 이해합니다.
---

# Datasets & Predictions \[Early Access\]

이 기능은 현재 얼리 액세스 단계에 있습니다. wandb.ai의 프로덕션 서비스에서 이를 사용하실 수 있지만[ 몇가지 제한 사항](https://docs.wandb.ai/v/ko/datasets-and-predictions#undefined-3)이 있습니다. API는 추후 변경될 수 있습니다. 저희는 여러분의 질문, 코멘트, 아이디어를 기다리고 있습니다! [feedback@wandb.com](mailto:feedback@wandb.com)으로 여러분의 생각을 알려주시기 바랍니다.

데이터는 모든 ML 워크플로우의 핵심입니다. 저희는 사용자들이 예시 수준으로 데이터세트 및 모델 평가의 시각화하고 쿼리할 수 있도록 W&B 아티팩트에 인상적인 새로운 기능을 추가하였습니다. 이 새로운 툴을 통해 데이터세트를 분석 및 이해하고, 모델 퍼포먼스를 측정 및 디버그\(debug\) 할 수 있습니다.

엔드투엔드 데모 해보기: [![](https://colab.research.google.com/assets/colab-badge.svg)](http://wandb.me/dsviz-demo-colab)

![&#xB370;&#xC774;&#xD130;&#xC138;&#xD2B8; &amp; &#xC608;&#xCE21; &#xB300;&#xC2DC;&#xBCF4;&#xB4DC;&#xC758; &#xBBF8;&#xB9AC; &#xBCF4;&#xAE30;](https://paper-attachments.dropbox.com/s_21D0DE4B22EAFE9CB1C9010CBEF8839898F3CCD92B5C6F38DBE168C2DB868730_1605673880422_image.png)

##  **작동방식**

저희의 목표는 일반적인 작업에 사용할 수 있는 창의적인 시각화와 함께, 뛰어난 확장성의 유연하고 구성 가능한\(configurable\) 툴을 제공하는 것입니다. 시스템은 다음으로 구성됩니다:

* 대형 wandb.Table 객체를 저장할 수 있는 기능으로, 이는 W&B 아티팩트 내부에 선택적으로 리치 미디어\(rich media\) \(예: 경계 상자\(bounding boxes\)가 있는 이미지\)를 포함합니다.
* 교차 아티팩트 파일 참조에 대한 지원 및 UI에 테이블을 함께 연결하는 기능. 예를 들어, 소스 이미지 및 라벨 복제 없이 기준 참 값\(ground-truth\) 데이터세트 아티팩트에 대한 일련의 경계 상자 예측을 로그 하는 데 사용됩니다.
* \[예정\] W&B 아티팩트에 저장된 테이블에 대한 대규모 쿼리에 대한 백엔드 API 지원
* z모든 새로운 “유형화된, 런타임-스왑 가능 UI 패널 아키택쳐”\(typed, run-time-swappable UI-panel architecture\). 이는 데이터 테이블 비교 및 그룹화 시에 표시되는 풍부한 시각화 및 차트를 강화합니다. 이 기능을 공개하여, 사용자가 W&B UI 어디서나 작업할 수 있는 사용자 지정 비주얼라이저\(visualizer\)를 추가할 수 있습니다.

## UI

이 [예시 프로젝트](https://wandb.ai/shawn/dsviz_demo/artifacts/dataset/train_results/18bab424be78561de9cd/files)를 열어 진행합니다. 이 예시는 저희 [demo colab](http://wandb.me/dsviz-demo-colab)에서 생성되었습니다.

로그된 테이블 및 미디어 객체를 시각화하려면, 아티팩트를 열고, **파일** 탭으로 이동한 후, 테이블 또는 객체를 클릭합니다. **그래프 보기**로 전환하여 프로젝트에서 아티팩트 및 실행이 연관된 방식을 확인합니다. **Explode**를 토글 하여 파이프라인에서 각 단계의 개별 수행을 확인합니다.

###  **테이블 시각화**

**파일** 탭을 열어 메인 시각화 UI를 확인합니다. [예시 프로젝트](https://wandb.ai/shawn/dsviz_demo/artifacts/dataset/train_results/18bab424be78561de9cd/files)에서 “train\_iou\_score\_table.table.json”를 클릭하여 시각화합니다. 데이터를 탐색하기 위해 테이블을 필터링, 그룹화 및 정렬할 수 있습니다.

![](.gitbook/assets/image%20%2899%29.png)

###  **필터링**

[mongodb 집계 언어\(aggregation language\)](https://docs.mongodb.com/manual/meta/aggregation-quick-reference/)로 지정되어 있으며, 중첩된 객체 \[aside: we don't actually use mongodb in our backend!\]로 쿼리할 수 있도록 지원하고 있습니다. 다음은 두 가지 예시입니다.

**“road”열에서 0.05를 초과하는 예시 찾기**

`{$gt: ['$0.road', 0.05]}` 

**하나 의상의 “car”경계 상자 예측이 있는 예시 찾기**

`{  
  $gte: [   
    {$size:   
      {$filter:   
        {input: '$0.pred_mask.boxes.ground_truth',  
          as: 'box',  
          cond: {$eq: ['$$box.class_id', 13]}  
        }  
      }  
    },  
  1]  
}`

###  **그룹화**

“dominant\_pred”로 그룹화합니다. 테이블이 그룹화되면 숫자 열이 자동으로 히스토그램으로 되는 것을 확인하실 수 있습니다.

![](https://paper-attachments.dropbox.com/s_21D0DE4B22EAFE9CB1C9010CBEF8839898F3CCD92B5C6F38DBE168C2DB868730_1605673736462_image.png)

\*\*\*\*

### **정렬**

**정렬**을 클릭하여 정렬할 열을 테이블에서 선택합니다.

###  **비교**

표에 있는 두 개의 아티팩트 버전을 비교합니다. 사이드바에서 “v3” 위에 마우스를 올려 놓고 “Compare” 버튼을 클릭합니다. 그러면 두 버전 모두의 예측이 테이블에 표시됩니다. 서로 겹쳐진 두 테이블을 생각해 보시기 바랍니다. 테이블은 수신 숫자 열\(incoming numeric columns\)에 대한 바 차트를 렌더링 하며, 각 표에 대한 하나의 막대로 비교합니다.

상단의 “Quick filters”\(빠른 필터\)을 사용하여 결과를 두 버전에만 있는 예시로 제한할 수 있습니다.

![](https://paper-attachments.dropbox.com/s_21D0DE4B22EAFE9CB1C9010CBEF8839898F3CCD92B5C6F38DBE168C2DB868730_1605673764298_image.png)

  
비교 및 그룹화를 동시에 시도해 보시기 바랍니다. “multi-histogram”\(다중 히스토그램\)을 얻으실 수 있으며, 다중 히스토그램에는 각 수신 테이블\(incoming table\) 당 하나의 색을 사용합니다.  


![](https://paper-attachments.dropbox.com/s_21D0DE4B22EAFE9CB1C9010CBEF8839898F3CCD92B5C6F38DBE168C2DB868730_1605673664913_image.png)

## Python API

엔드투엔드 예시에 대한 [demo colab](http://wandb.me/dsviz-demo-colab)을 사용해 보시기 바랍니다.

데이터세트 및 예측을 시각화하기 위해 리치 미디어를 아티팩트로 로그합니다. W&B 아티팩트에 원본 파일\(raw files\)하는 것뿐만 아니라, 이제 wandb APl에서 제공하는 다른 리치 미디어 유형을 저장, 검색 및 시각화할 수 있습니다.

현재 다음의 유형을 지원합니다:

* wandb.Table\(\)
* wandb.Image\(\)

 추가적인 미디어 유형 지원은 예정 중입니다.

### **새 아티팩트 방법**

 아티팩트 객체에 대한 두 가지 새로운 방법이 있습니다

`artifact.add(object, name)`

* 아티팩트에 미디어 객체를 추가합니다. 현재 지원되는 유형은 wandb.Table 및 wandb.Image이며, 추가 지원 예정 중입니다.
* 이를 통해 차일드 미디어\(child media\) 객체 및 에셋\(assets\) \(예: 원본 ‘.png’ 파일\) 을 아티팩트에 반복적으로 추가합니다. 

`artifact.get(name)`

* 저장된 아티팩트에서 재구성된 미디어 객체를 반환합니다.

이러한 방법은 대칭적입니다. .add\(\)를 사용하여 아티팩트에 객체를 저장할 수 있으며, .get\(\)을 사용하여 필요한 모든 머신에서 정확히 동일한 객체를 다시 가져올 수 있는지 확인하시기 바랍니다.

### **wandb.\* media objects**

`wandb.Table`

테이블은 데이터세트 및 예측 시각화의 핵심입니다. 데이터세트를 시각화하기 위해, 데이터세트를 wandb.Table에 놓고, 필요에 따라 wandb.Image 객체, 배열\(arrays\), 사전, 스트링 및 숫자를 추가하고, 그다음 테이블을 아티팩트에 추가합니다. 현재, 각 테이블은 50,000개의 행으로 제한됩니다. 원하는 만큼 많은 테이블을 아티펙트에 로그할 수 있습니다.

다음의 예시 코드는 Keras cifar10 실험 데이터세트의 1000개의 이미지 및 라벨을 아티팩트 내부의 wandb.Table로 저장합니다.

```python
import tensorflow as tf
import wandb

classes = ['airplane', 'automobile', 'bird', 'cat',
           'deer', 'dog', 'frog', 'horse', 'ship', 'truck']
_, (x_test, y_test) = tf.keras.datasets.cifar10.load_data()

wandb.init(job_type='create-dataset') # start tracking program execution

# construct a table containing our dataset
table = wandb.Table(('image', 'label'))
for x, y in zip(x_test[:1000], y_test[:1000]):
    table.add_data(wandb.Image(x), classes[y[0]])

# put the table in an artifact and save it
dataset_artifact = wandb.Artifact('my-dataset', type='dataset')
dataset_artifact.add(table, 'dataset')
wandb.log_artifact(dataset_artifact)
```

이 코드를 실행한 후, W&B UI에서 테이블을 시각화할 수 있습니다. 아티팩트 파일 탭에서 “dataset.table.json”를 클릭 합니다. “label”별로 그룹화하여 “image”열에 있는 각 클래스의 예시를 얻을 수 있습니다.

  
`wandb.Image`

[wandb.log 문서](https://docs.wandb.com/library/log#images-and-overlays)에 설명된 바와 같이 wandb.Image 객체를 구성할 수 있습니다.

wandb.Image를 사용하면 위의 문서에 명시된 것처럼 분할 마스크\(segmentation masks\) 및 경계 상자를 이미지에 첨부할 수 있습니다. wandb.Image\(\) 객체를 아티팩트에 저장하려는 경우, 한 가지 변경 사항이 있습니다: 이전에는 각 wandb.Image에 저장되어야 했던 “class\_lables”를 제외했습니다.

이제 다음과 같이 경계 상자 또는 분할 마스크를 사용하는 경우 클래스 라벨을 별도로 생성해야 합니다.

```python
class_set = wandb.Classes(...)
example = wandb.Image(<path_to_image_file>, classes=class_set, masks={
            "ground_truth": {"path": <path_to_mask>}})
```

또한, 다른 아티팩트에 로그된 wandb.Image를 참조하는 wandb.Image를 구성\(construct\)할 있습니다. 이를 통해 기본 이미지\(underlying image\) 중복을 피하기 위해 교차-아티팩트-파일-참조\(cross-artifact-file-reference\)를 사용합니다.

```python
artifact = wandb.use_artifact('my-dataset-1:v1')
dataset_image = artifact.get('an-image')  # if you've logged a wandb.Image here 
predicted_image = wandb.Image(dataset_image, classes=class_set, masks={
            "predictions": {"path": <path_to_mask>}})
```

  
`wandb.Classes`

클래스 id \(숫자\)에서 라벨 \(스트링\)로 매핑을 정의하는데 사용됩니다:

```python
CLASSES = ['dog', 'cat']
class_set = wandb.Classes([{'name': c, 'id': i} for i, c in enumerate(CLASSES)])
```



`wandb.JoinedTable`

W&B UI에 두 테이블의 결합\(join\)을 렌더링 하도록 명령하는데 사용합니다. 테이블은 다른 아티팩트에 저장될 수 있습니다.

```python
jt = wandb.JoinedTable(table1, table2, 'id')
artifact.add(jt, 'joined')
```

##  **엔드투엔드 예시**

다음의 내용을 다루는 엔드투엔드 예시에 대한 [colab notebook](http://wandb.me/dsviz-demo-colab)을 사용해 보시기 바랍니다:

* 데이터세트 구성\(constructions\) 및 시각화
* 모델 훈련
*  데이터세트에 대한 예측 로깅 및 시각화하기

##  **자주 묻는 질문**

**저는 훈련을 위해 데이터세트를 이진 포맷\(binary format\)으로 패킹\(pack\) 합니다. W&B 데이터세트 아티팩트와 어떤 관련이 있나요?**

 다음과 같은 몇 가지 방법을 시도할 수 있습니다:

1. 데이터세트에 대한 기록 시스템으로 wandb.Table 포맷을 사용합니다. 여기서 다음 두 가지 중 하나를 수행하실 수 있습니다:
   1. 훈련 시간에 W&B 포맷 아티팩트에서 패킹된 포맷을 추출\(derive\) 합니다
   2. 또는, 패킹된 포맷 아티팩트를 생성하는 파이프라인 단계를 가지고, 해당 아티팩트에서 훈련을 수행합니다.
2. 패킹된 포맷과 wandb.Table을 동일 아티팩트에 저장합니다
3. 패킹된 모팻을 지정한 작업을 생성하고 wandb.Table 아티팩트를 로그합니다.

\[참고: 알파 단계에서는, W&B 아티팩트에 저장된 테이블에 50k 행 제한이 있습니다\]

모델 예측 쿼리 및 시각화하려는 경우, 훈련 단계를 통한 예시 ID 전달 방법에 대해서 고려해야 사용자의 예측 테이블이 소스 데이터세트 테이블에 다시 합쳐질 수 있습니다. 몇 가지 방법에 대하여 저희 링크된 예시를 참조하시기 바랍니다.

저희는 추후 시간이 지남에 따라 공통된 포맷에 대한 컨버터\(converters\), 더 많은 예시 및 널리 사용되는 프레임워크와의 긴밀한 통합을 제공하겠습니다.

##  **현재 제한 사항**

이 기능은 현재 얼리 액세스 단계에 있습니다. wandb.ai의 프로덕션 서비스에서 이를 사용하실 수 있지만, 몇 가지 제한 사항이 있습니다. API는 추후 변경될 수 있습니다. 저희는 여러분의 질문, 코멘트, 아이디어를 기다리고 있습니다! [feedback@wandb.com](mailto:feedback@wandb.com)으로 여러분의 생각을 알려주시기 바랍니다.

* 스케일\(scale\): 아티팩트에 로그된 테이블은 현재 50,000개의 행으로 제한됩니다. 향후 테이블당 100m+ 행을 처리하는 것을 목표로, 각 버전 출시마다 이를 증가시킬 예정입니다.
* 현재 지원되는 wandb.\* 미디어 유형:
  * wandb.Table
  * wandb.Image
* W&B에 쿼리 및 보기\(views\)를 저장하고 지속하는 방법이 없습니다.
* W&B 리포트에 시각화를 추가할 수 있는 방법이 없습니다.

##  **당면 과제**

* 진행 중인 많은 UX & UI 개선사항
* 테이블당 행 제한 증가
* 테이블 저장을 위해 columnar binary 포맷 \(Parquet\) 사용
* wandb.Table 외에 데이터프레임 및 기타 일반적인 Python 테이블 포맷 처리하기
* 심층 집계\(aggregation\) 및 분석을 지원하는 보다 강력한 쿼리 시스템 추가
* 더 많은 미디어 유형 지원
* 보기 / 작업공간 저장을 통해 UI 상태 유지 기능 추가
* 동료와 공유를 위한 W&B 리포트에 시각화 & 분석을 저장하는 기능
* 재라벨링\(relabeling\) 및 기타 워크플로우에 대한 쿼리 및 부분집합\(subsets\) 저장 기능
* Python에서 쿼리 하는 기능
* 더 큰 테이블 데이터를 사용하는 다른 시각화 추가 기능 \(예: 클러스터 비주얼라이저\(visualizer\)
* 완전한 사용자 지정 시각화를 위한 사용자 작성\(user-authored\) 패널 지원

