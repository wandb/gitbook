---
description: '실행이 충돌, 완료 또는 wandb.alert()을 호출 할 때 마다 Slack 알림을 받으실 수 있습니다.'
---

# Alerts

W&B는 실행이 충돌, 완료 또는 [wandb.alert\(\)](https://docs.wandb.com/library/wandb.alert)을 호출 할 때마다 Slack에 경보를 게시할 수 있습니다.

**사용자 수준 경보**

여러분이 실행한 실행이 충돌, 완료 또는 wandb.alert\(\)을 호출할 때마다 경보를 받으시려면, 사용자 수준 경보를 설정하실 수 있습니다. 사용자 수준 경보는 팀 프로젝트를 포함한 모든 프로젝트에 적용되며, 사용자가 깨진 또는 실패한 실행이 있는 경우에 언제든지 작동합니다.

Personal Slack Integration섹션 아래의 user settings 페이지에서 Connect Slack 버튼을 선택하고, W&B가 경보를 게시할 채널을 승인합니다. slackbot 채널은 경보를 비공개로 유지하기에 아주 좋습니다. W&B가 경보를 게시할 Slack 채널을 변경해야 하는 경우, Disconnect Slack 버튼을 선택하고 여러분이 선택한 새 채널을 사용하는 Slack과 다시 연결합니다.

![](../../.gitbook/assets/demo-connect-slack.png)

일단 Slack이 연결되면, 여러분께서 자유롭게 경보를 활성화 및 비활성화 할 수 있습니다.

### Changing Slack Channels

또한 이메일을 통해 알림을 수신하실 수도 있습니다. W&B는 여러분의 계정과 연결된 이메일을 사용해서 알림을 보냅니다.

