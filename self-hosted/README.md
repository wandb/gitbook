---
description: 민감한 데이터를 포함한 프로젝트를 위한 자체 호스팅 설치
---

# Self Hosted

W&B Local은 [Weights & Biases](https://app.wandb.ai/)의 자체 호스팅 버전입니다. 이를 통해서, 기업 머신러닝 팀을 위한 공동 실험\(collaborative experiment\) 추적을 수행하실 수 있으며, 모든 훈련 데이터 및 메타데이터를 여러분의 조직 네트워크 내에 보관할 수 있는 방안을 제공합니다.

  [W&B Local 테스트용 데모 요청하기 →](https://www.wandb.com/demo)​ 

저희는 또한 여러분의 회사의 AWS 또는 GCP 계정 내에서 전적으로 확장 가능한 인프라를 운영하는 [W&B 기업 클라우드](https://docs.wandb.com/self-hosted/cloud)도 제공하고 있습니다. 이 시스템은 모든 사용 수준으로 확장하실 수 있습니다.  


## **특징**

* 무제한 실행, 실험 및 리포트
* 회사 네트워크에서 데이터를 안전하게 보관
* 사용자 기업의 인증 시스템과 통합
* W&B 엔지니어링 팀에서 최고의 지원 제공

자체 호스팅 서버는 배포가 쉬운 단일 도커 이미지\(Docker image\)입니다. 여러분의 W&B 데이터는 영구 볼륨\(persistent volume\) 또는 외부 데이터베이스에 저장되므로, 여러 컨테이너 버전에 걸쳐 데이터를 보존할 수 있습니다.

##  **서버 요구 사항**

W&B 자체 호스팅 서버는 적어도 4코어 및 8GB 메모리인 인스턴스를 필요로 합니다.

##  **자체 호스팅 리소스**

{% page-ref page="setup.md" %}

{% page-ref page="local.md" %}

{% page-ref page="configuration.md" %}

{% page-ref page="local-common-questions.md" %}

{% page-ref page="cloud.md" %}

