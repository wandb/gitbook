---
description: '실행이 충돌, 완료 또는 wandb.alert()을 호출 할 때 마다 Slack 알림을 받으실 수 있습니다.'
---

# Alerts

W&B는 실행이 충돌, 완료 또는 [wandb.alert\(\)](https://docs.wandb.ai/v/ko/library/wandb.alert)을 호출 할 때마다 Slack에 경보를 게시할 수 있습니다.

Jupyter Notebook 환경에서는 모든 셀 실행 시 경보 알림을 방지하기 위해 완료된 실행에 대한 경보가 비활성화됩니다. 대신 Jupyter Notebook 환경에서 wandb.alert\(\)를 사용하십시오.

**사용자 수준 경보**

여러분이 실행한 실행이 충돌, 완료 또는 wandb.alert\(\)을 호출할 때마다 경보를 받으시려면, 사용자 수준 경보를 설정하실 수 있습니다. 사용자 수준 경보는 실행을 진행하는 모든 프로젝트에 적용되며, 개인 및 팀 프로젝트 모두를 포함합니다.

[**사용자 설정**](https://wandb.ai/settings)**에서 다음을 진행합니다,**

* **경고 섹션**까지 스크롤을 내립니다.
* **Connect Slack** 버튼을 클릭하여, W&B가 경보를 게시할 채널을 선택합니다**. Slackbot** 채널은 경보를 비공개로 유지하기에 아주 좋으므로, Slackbot 채널을 추천합니다**.**
* W&B 가입시에 사용한 이메일 주소로 이메일이 전송됩니다. 모든 경보가 귀하의 받은 편지함을 가득 채우지 않고, 설정한 폴더에 들어가도록 이메일에 필터를 설정하시기 바랍니다. 

![](../../.gitbook/assets/demo-connect-slack.png)

**팀 수준 경보**

팀 관리자는 팀 설정 페이지\(wandb.ai/teams/your-team\)에서 팀 경보를 설정할 수 있습니다. 이러한 경보는 팀의 모든 사용자에게 적용됩니다. Slackbot 채널은 알림을 비공개로 유지하므로 사용하는 것이 좋습니다.

### **Slack 채널 변경하기**

게시할 채널을 변경하려면 Disconnect Slack을 클릭한 다음 다른 대상 채널을 선택하여 연결합니다.  


