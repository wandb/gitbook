---
description: wandb가 자동으로 로그하는 메트릭
---

# System Metrics

 `wandb`는 매 2초마다 자동으로 시스템 메트릭을 로그하며, 이는 30초 동안 평균을 냅니다. 메트릭에는 다음이 포함됩니다.

* CPU 사용률
* 시스템 메모리 사용률
* Disk I/O 사용률
* 네트워크 트래픽 \(송/수신된 bytes\)
* GPU 사용률
* GPU 온도
* 메모리 액세스에 소요된 GPU 시간 \(샘플 시간의 백분율로서의\)
* 할당된 GPU 메모리

GPU 메트릭은 [nvidia-ml-py3](https://github.com/nicolargo/nvidia-ml-py3/blob/master/pynvml.py)를 사용하여 디바이스별로 수집됩니다. 이러한 메트릭을 해석하고 모델 퍼포먼스를 최적화하는 방법에 대한 자세한 정보는 [Lambda Labs의 유용한 블로그 포스트](https://lambdalabs.com/blog/weights-and-bias-gpu-cpu-utilization/)를 참조하시기 바랍니다.

