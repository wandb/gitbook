# SageMaker

## **SageMaker 통합**

 W&B는 [Amazon SageMaker](https://aws.amazon.com/sagemaker/)와 통합되어, 자동적으로 초매개변수를 읽고, 분배된 실행 그룹화, 체크포인트에서 실행 재개 등을 수행합니다.

###  **인증**

 W&B는 훈련 대본과 관련된 `secrets.env` 라는 이름의 파일을 탐색하여 `wandb.init()`이 요청되면 환경으로 로드합니다. 실험을 실행할 때 사용하는 스크립트에서 `wandb.sagemaker_auth(path="source_dir")`을 요청하여 secrets.env 파일을 생성하실 수 있습니다. 이 파일을 여러분의 `.gitignore`에 추가하셔야 합니다!  


### **기존 추정기\(Estimator\)**

 SageMakers 사전 구성된 추정기\(preconfigured estimators\) 중 하나를 사용하시는 경우, `requirements.txt` 를 wandb가 포함된 여러분의 소스 디렉토리에 추가하셔야 합니다.

```text
wandb
```

 Python 2를 실행하는 추정기\(estimator\)을 사용하는 경우, wandb를 설치하시기 전에 [wheel](https://pythonwheels.com/)에서 직접 psutil을 설치하셔야 합니다.

```text
https://wheels.galaxyproject.org/packages/psutil-5.4.8-cp27-cp27mu-manylinux1_x86_64.whl
wandb
```

 [GitHub](https://github.com/wandb/examples/tree/master/examples/pytorch/pytorch-cifar10-sagemaker)에서 전체 예시를 확인하실 수 있으며, 자세한 내용은 저희 [블로그](https://www.wandb.com/blog/running-sweeps-with-sagemaker)에서 확인하실 수 있습니다.

