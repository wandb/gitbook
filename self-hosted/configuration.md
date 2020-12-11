---
description: W&B 로컬 서버 설치를 구성하는 방법
---

# Advanced Configuration

여러분의 W&B 로컬 서버는 부팅 시 바로 사용하실 수 있습니다. 하지만, 일단 작동을 시작하면, 여러분 서버의 `/system-admin` 페이지에서 여러 고급 구성 옵션을 사용 할 수 있습니다. 더 많은 사용자와 평가판 라이선스를 요청하여 더 많은 사용자와 팀을 지원하려면 [contact@wandb.com](mailto:contact@wandb.com)로 이메일을 보내주시기 바랍니다.

##  **코드로 구성하기**

모든 구성 설정은 UI를 통해 설정할 수 있지만, 이러한 구성 옵션을 코드를 통해 관리하시려면, 다음의 환경변수를 설정하실 수 있습니다:

| Environment Variable | Description |
| :--- | :--- |
| LICENSE | 여러분의 wandb/local 라이선스 |
| MYSQL | MySQL 연결 스트링 |
| BUCKET | 데이터 저장용 S3 / GCS 버킷 |
| BUCKET\_QUEUE | 객체 생성 이벤트에 대한 The SQS / Google PubSub 대기열 |
| NOTIFICATIONS\_QUEUE | 실행 이벤트를 게시할 SQS 대기열 |
| AWS\_REGION | 여러분의 버킷이 있는 AWS 영역 |
| HOST |  인스턴스의 FQD. 즉, [https://my.domain.net](https://my.domain.net/) |
| AUTH0\_DOMAIN | 테넌트\(tenant\)의 Auth0 도메인 |
| AUTH0\_CLIENT\_ID | 어플리케이션의 Auth0 클라이언트 ID |

##  **인증**

기본적으로 W&B 로컬 서버는 최대 4명의 사용자를 지원하는 수동 사용자 관리와 함께 실행됩니다. 또한, wandb/local 정식 라이선스 버전에서는 Auth0를 사용하는 SSO를 잠금 해제 할 수 있습니다.

여러분의 서버는 [Auth0](https://auth0.com/)에서 지원하는 모든 인증 공급자\(authentication provider\)를 지원합니다. 여러분 팀의 통제하에 있는 고유의 Auth0 도메인 및 어플리케이션을 설정하셔야 합니다.

Auth0 앱을 생성 한 후, 여러분의 Auth0 콜백을 W&B 서버의 호스트에 설정해야 합니다. 기본값으로 서버는 호스트에서 재공하는 공용 또는 개인 IP 주소의 http를 지원합니다. 또한, 선택하신 경우 DNS 호스트 이름과 SSL 인증서를 구성하실 수도 있습니다.

* 콜백 URL을 다음에 설정: `http(s)://YOUR-W&B-SERVER-HOST`
* 허용된 Web Origin을 다음에 설정 `http(s)://YOUR-W&B-SERVER-HOST`
* 로그아웃 URL을 다음에 설정: `http(s)://YOUR-W&B-SERVER-HOST/logout`

![Auth0 &#xC124;&#xC815;](../.gitbook/assets/auth0-1.png)

 Auth0 앱에서 클라이언트 ID와 도메인을 저장합니다.

![Auth0 &#xC124;&#xC815;](../.gitbook/assets/auth0-2.png)

T 그런 다음, W&B 설정 페이지 `http(s)://YOUR-W&B-SERVER-HOST/admin-settings`로 이동합니다. “Customize Authentication with Auth0 \(Auth0로 인증 사용자 지정하기\)” 옵션을 활성화 하고, Auth0 앱에서 클라이언트 ID와 도메인을 입력합니다.

![&#xAE30;&#xC5C5; &#xC778;&#xC99D; &#xC124;&#xC815;](../.gitbook/assets/enterprise-auth.png)

마지막으로 “Update settings and restart W&B \(설정 업데이트 및 W&B 재실행\)”를 누릅니다.

##  **파일 스토리지**

기본값으로, W&B 기업 서버는 인스턴스를 프로비저닝할 때 설정한 용량을 통해 로컬 데이터 디스크에 파일을 저장합니다. 무제한 파일 스토리지를 지원하기 위해, S3 호환 API와 함께 외부 클라우드 파일 스토리지를 사용하도록 서버를 구성할 수 있습니다.

### **Amazon 웹 서비스**

AWS S3 버킷을 W&B 파일 스토리지 백엔드로 사용하시려면, 버킷에서 객체 생성\(object creation\) 알림을 수신하도록 설정된 SQS 대기열과 함께 해당 버킷을 생성해야 합니다. 여러분의 인스턴스가 이 대기열에서 읽을 수 있는 권한이 필요합니다.

 **SQS 대기열 생성하기**

 우선, SQS 표준 대기열을 생성합니다. `GetQueueUrl` 뿐만 아니라 `SendMessage` 및 `ReceiveMessage` 작업에 대한 모든 주체에 대한 권한을 추가합니다. \(원하시는 경우, 고급 정책 문서를 사용하여 이를 추가로 잠글 수 있습니다.\)

![&#xAE30;&#xC5C5; &#xD30C;&#xC77C; &#xC2A4;&#xD1A0;&#xB9AC;&#xC9C0; &#xC124;&#xC815;](../.gitbook/assets/sqs-perms.png)

**S3 버킷 및 버킷 알림 생성하기**

그런 다음, S3 버킷을 생성합니다. 콘솔의 bucket properties\(버킷 속성\) 페이지의 “Advanced Settings\(고급 설정\)”의 “Events\(이벤트\)” 섹션에서 “Add notification\(알림 추가\)”를 클릭하고, 이전에 구성한 SQS 대기열에 보낼 모든 개체 생성 이벤트를 구성합니다.

![&#xAE30;&#xC5C5; &#xD30C;&#xC77C; &#xC2A4;&#xD1A0;&#xB9AC;&#xC9C0; &#xC124;&#xC815;](../.gitbook/assets/s3-notification.png)

CORS 액세스를 활성화합니다. CORS 구성은 다음과 같아야 합니다:

```markup
<?xml version="1.0" encoding="UTF-8"?>
<CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
<CORSRule>
    <AllowedOrigin>http://YOUR-W&B-SERVER-IP</AllowedOrigin>
    <AllowedMethod>GET</AllowedMethod>
    <AllowedMethod>PUT</AllowedMethod>
    <AllowedHeader>*</AllowedHeader>
</CORSRule>
</CORSConfiguration>
```

 **W&B 서버 구성하기**

마지막으로, W&B 설정 페이지 `http(s)://YOUR-W&B-SERVER-HOST/admin-settings`로 이동합니다. “Use an external file storage backend\(외부 파일 스토리지 백엔드 사용\)” 옵션을 활성화하고, s3 버킷, 영역, SQS 대기열을 다음 포맷으로 작성합니다:

* **File Storage Bucket\(파일 스토리지 버킷\)**: `s3://<bucket-name>`
* **File Storage Region\(파일 스토리지 영역\)** `<region>`
* **Notification Subscription \(알림 구독\)**:: `sqs://<queue-name>`

![AWS &#xD30C;&#xC77C; &#xC2A4;&#xD1A0;&#xB9AC;&#xC9C0; &#xC124;&#xC815;](../.gitbook/assets/aws-filestore.png)

“update settings and restart W&B\(설정 업데이트 및 W&B 재실행\)”을 눌러 새 설정을 적용합니다.

### **Google 클라우드 플랫폼**

GCP 스토리지 버킷을 W&B 파일 스토리지 백엔드로 사용하시려면, 버킷에서 객체 생성\(object creation\) 메시지를 수신하도록 설정된 pubsub 항목\(topic\) 및 구독\(subscription\)과 함께 버킷을 생성해야 합니다.

 **Pubsub 항목 및 구독 생성하기**

GCP 콘솔에서 Pub/Sub &gt; Topics으로 이동하고, “Create topic\(항목 생성\)”을 클릭합니다. 이름을 선택하고 항목을 생성합니다.

그런 다음 페이지 하단의 구독테이블에서 “Create subscription\(구독 생성\)”을 클릭합니다. 이름을 선택하고, 전달 유형\(Delivery Type\)을 “Pull\(풀\)”로 설정합니다. “Create\(생성\)”를 클릭하세요.

인스턴트가 실행 중인 서비스 계정 또는 계정이 이 구독에 액세스 할 수 있는지 확인하십시오.

 **스토리지 버킷 생성하기**

GCP 콘솔에서 Storage &gt; Browser로 이동하고, “Create bucket\(버킷 생성\)”을 클릭합니다. “Standard\(표준\)” 스토리지 클래스를 선택하셔야 합니다.

인스턴트가 실행 중인 서비스 계정 또는 계정이 이 버킷에 액세스 할 수 있는지 확인하십시오.

 **Pubsub 알림 생성하기**

스토리지 버킷\(Storage Bucket\)에서 Pubsub 항목\(Topic\)으로의 알림 스트림 생성은 안타깝게도 콘솔에서만 수행하실 수 있습니다. `gsutil`가 설치되어 있는지 및 정확한 GCP Project\(프로젝트\)에 로그인 되어있는지 확인하시고, 다음을 실행합니다:

```bash
gcloud pubsub topics list  # list names of topics for reference
gsutil ls                  # list names of buckets for reference

# create bucket notification
gsutil notification create -t <TOPIC-NAME> -f json gs://<BUCKET-NAME>
```

[클라우드 스토리지 웹사이트에서 추가 참고자료를 확인하실 수 있습니다.](https://cloud.google.com/storage/docs/reporting-changes)

 **서명 권한 추가하기**

서명된 파일 URL을 생성하려면, W&B 인스턴스에도 GCP의 `iam.serviceAccounts.signBlob` 권한이 필요합니다. 인스턴스가 실행중인 서비스 계정 또는 IAM 구성원\(member\)에 `Service Account Token Creator`를 추가하여 이를 추가할 수 있습니다.

 **W&B 서버 구성하기**

 마지막으로 W&B 설정 페이지의 `http(s)://YOUR-W&B-SERVER-HOST/admin-settings`로 이동합니다. “Use an external file storage backend\(외부 파일 스토리지 백엔드 사용\)” 옵션을 활성화하고, s3 버킷, 영역, 및 SQS 대기열을 다음 포맷으로 작성합니다.

* **File Storage Bucket\(파일 스토리지 버킷\)**: `gs://<bucket-name>`
* **File Storage Region\(파일 스토리지 영역\)**: : blank
* **Notification Subscription\(알림 구독\)**: `pubsub:/<project-name>/<topic-name>/<subscription-name>`

![GCP &#xD30C;&#xC77C; &#xC2A4;&#xD1A0;&#xB9AC;&#xC9C0; &#xC124;&#xC815;](../.gitbook/assets/gcloud-filestore.png)

마지막으로 “update settings and restart W&B \(설정 엎데이트 및 W&B 재실행\)”를 누릅니다.



