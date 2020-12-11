---
description: 로컬로 호스팅된 버전의 앱 설정에 대해 자주 묻는 질문
---

# Local FAQ

## **제 서버가 인터넷에 연결되어 있어야 하나요?**

아닙니다. wandb/local은 에어 갭\(air gapped\)된 환경에서 실행될 수 있습니다. 단, 유일한 요구사항으로 모델을 훈련 시키는 머신은 네트워크를 통해서 이 서버에 연결할 수 있어야 합니다.

##  **제 데이터는 어디에 저장되나요?**

 기본 도커 이미지\(default docker image\)는 컨테이너의 내부에서 MySQL및 Minio를 실행하고 모든 데이터를 `/vol`의 하위 폴더\(sub folders\)에 작성합니다. 라이선스를 취득하여 외부 MySQL 및 객체 스토리지\(Object Storage\)를 구성하실 수 있습니다. 자세한 내용은 [contact@wandb.com](mailto:contact@wandb.com)으로 이메일을 보내주시기 바랍니다.

##  **업그레이드를 얼마나 자주 출시하나요?**

저희는 적어도 한 달에 한 번은 업그레이드된 버전의 서버를 출시하기 위해 최선을 다하고 있습니다.

##  **제 서버가 다운되면 어떻게 되나요?**

진행 중인 실험은 백오프 재시도 루프\(backoff retry loop\)로 들어가 24시간 동안 연결을 계속 시도합니다..

## **이 서비스의 스케일링 특성은 무엇인가요?**

 외부 MySQL 스토어가 없는 wandb/local의 단일 인스턴스는 한 번에 추적되는 최대 10개의 동시발생 실험으로 스케일링\(scale\) 됩니다. 외부 MySQL 스토어와 연결된 인스턴스는 100개의 동시발생 실행으로 스케일링\(scale\)됩니다. 더 많은 동시발생 실험을 추적해야 하는 경우, 멀티 인스턴스 고가용성 설치 옵션\(multi instance high availability installation options\)에 대해서 문의사항이 있으시면 저희에게 [contact@wandb.com](mailto:contact@wandb.com)로 메모를 남겨주시기 바랍니다.

## **인스턴스에 액세스할 수 없는 경우, 어떻게 공장 초기화\(factory reset\)를 해야 하나요?**

 인스턴스에 연결 할 수 없는 경우, 로컬을 시작할 때 LOCAL\_RESTORE 환경 변수를 설정하여 인스턴스를 복원 모드로 전환할 수 있습니다. 저희 cli를 사용하여 wandb 로컬을 시작하는 경우, `wandb local -e LOCAL_RESTORE=true`를 사용하여 시작하실 수 있습니다. 인스턴스에 액세스하기 위한 임시 사용자 이름\(temporary username\) / 비밀번호\(password\)에 대한 스타트업에 출력된 로그를 확인하시기 바랍니다.

## **로컬을 사용한 후 클라우드로 다시 전환하려면 어떻게 해야 되나요?**

[저희의 클라우드 호스팅 솔루션으로의 보고 메트릭\(reporting metrics\)으로 머신을 복원하시려면, 다음을 실행하세요](https://docs.wandb.com/self-hosted/cloud): `wandb login --host=https://api.wandb.ai`

