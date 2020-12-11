---
description: 도커(Docker)를 사용하는 여러분의 머신에서 Weights and Biases를 실행하세요
---

# Local

## **서버 시작하기**

W&B 서버를 로컬로 실행하시려면, [도커](https://www.docker.com/products/docker-desktop)를 설치하셔야 합니다. 그 후 다음을 실행합니다:

```text
wandb local
```

이 이면에서 wandb 클라이언트 라이브러리는 [wandb/local](https://hub.docker.com/repository/docker/wandb/local) 도커 이미지를 실행하고 있으며, 포트 8080을 호스트에 전달하고, 여러분의 머신을 구성하여 저희의 호스트 클라우드 대신 메트릭을 여러분의 로컬 인스턴스에 전송합니다.

```text
docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

### **중앙 집중식 호스팅**

localhost에서 wandb를 실행하는 것은 초기 실험에는 좋지만, wandb/local의 협력 기능을 활용하려면 서비스를 중앙 서버에서 호스팅 하셔야 합니다. 중앙 집중식 서버 설정 방법은 [설정](https://docs.wandb.com/self-hosted/setup) 섹션에서 확인하실 수 있습니다.

###  **기본 구성**

wandb 로컬을 실행하면 로컬 머신이 메트릭을 [http://localhost:8080](http://localhost:8080/)으로 푸시하도록 구성됩니다. 다른 포트에서 로컬을 호스팅하려는 경우, --port 전달인자를 `wandb local`로 전달하실 수 있습니다. 로컬 인스턴스로 DNS를 구성하신 경우, 메트릭을 보고할 머신에서 다음을 실행하실 수 있습니다: wandb login `--host=http://wandb.myhost.com`. 또한 `WANDB_BASE_URL` 환경 변수를 로컬 인스턴스에 보고할 머신의 호스트 또는 IP로 설정하실 수 있습니다. 자동화된 환경에서는, 설정 페이지의 api 키 내에서 `WANDB_API_KEY` 환경 변수를 설정하시는 것이 좋습니다. 저희의 클라우드 호스팅 솔루션으로의 보고 메트릭\(reporting metrics\)으로 머신을 복원하시려면, 다음을 실행하세요: `wandb login --host=https://api.wandb.ai`.

###  **인증**

_wandb/local의 기본설치는 기본값 사용자 local@wandb.com으로 시작합니다. 기본값 비밀번호는_ **perceptron**입니다. 프론트엔드가 자동으로 이 사용자로 로그인을 시도하고, 여러분께 비밀번호 재설정 여부를 확인합니다.라이선스가 없는 버전의 wandb를 하시는 경우 최대 4명의 사용자까지 생성하실 수 있습니다. `http://localhost:8080/admin/users`의 wandb/local의User Admin페이지에서 사용자를 구성하실 수 있습니다.

###  **지속성**

**업그레이드**

저희는 정기적으로 새 버전의 wandb/local을 도커허브\(dockerhub\) 더하고 있습니다. 업그레이드 하시려면 다음을 실행하세요:

```text
$ wandb local --upgrade
```

수동으로 인스턴스\(instance\)를 업그레이드 하시려면 다음을 실행하세요

```text
$ docker pull wandb/local
$ docker stop wandb-local
$ docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

###  **라이선스 얻기**

팀 구성, 외부 스토리지 사용 또는 wandb/local을 Kubernests 클러스터에 배포하는 데 관심이 있으시다면 저희에게 [contact@wandb.com](mailto:contact@wandb.com)로 이메일을 보내주시기 바랍니다

