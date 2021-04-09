# Technical FAQ

### **이것이 제 훈련 프로세스에 어떤 영향을 미치나요?**

훈련 스크립트에서 `wandb.init()`가 호출된 경우, API 호출을 통하여 서버에 실행 객체를 생성합니다. 메트릭을 스트리밍 및 수집하기 위해 새로운 프로세스가 시작되므로, 모든 스레드\(threads\) 및 로직\(logic\)은 기본 프로세스\(primary process\)에 포함시키지 않습니다. 스크립트는 정상적으로 실행되며 로컬파일에 작성되며, 개별 프로세스는 이를 시스템 메트릭과 함께 서버로 스트리밍합니다. 훈련 디렉토리에서 `wandb off`를 실행하거나 **WANDB\_MODE 환경 변수를 “dryrun”으로 설정하여 항상 스트리밍을 끌 수 있습니다.**

###  **wandb가 고장 나는 경우, 제 훈련 실행이 고장 날 수도 있나요?**

 여러분의 훈련 실행에 지장을 주지 않는 것은 저희에게 매우 중요한 사안입니다. 저희는 별도의 프로세스에 wandb를 실행하여 wandb가 제대로 작동하지 않는 경우, 여러분의 훈련이 계속 실행되도록 합니다. 인터넷 연결이 끊긴 경우, wandb는 wandb.com으로 데이터 전송을 계속 재시도합니다.

### **wandb가 제 훈련 속도를 느리게 할까요?**

wandb는 여러분이 정상적으로 사용할 경우 여러분의 훈련 퍼포먼스에 거의 영향을 미치지 않습니다. wandb를 정상적으로 사용한다는 것은 초당 1회 미만의 로깅과 각 단계에서 수 메가바이트 미만의 로깅을 의미합니다. Wandb는 별도의 프로세스에서 실행되며 함수 호출\(function calls\)은 차단되지 않으므로, 네트워크가 잠시 중단되거나 디스크에 간헐적 읽기 쓰기 문제가 있는 경우, 여러분의 퍼포먼스에 영향을 미치지 않습니다. 대량의 데이터가 빠르게 로그 될 수 있으며, 이 경우, 디스크 I/O 문제가 발생할 수 있습니다. 궁금하신 사안이 있으실 경우, 저희에게 언제든지 문의해 주시기 바랍니다.

###  **오프라인에서 wandb를 실행할 수 있나요?**

 오프라인 머신에서 훈련 중이며, 나중에 결과를 서버에 업로드하고 싶으신 경우, 다음의 기능을 사용하실 수 있습니다.

1. 환경변수 `WANDB_MODE=dryrun` 설정하여 메트릭을 로컬로 저장합니다. 인터넷은 필요하지 않습니다.
2. 준비되면 `wandb init`을 디렉토리에 실행하여 프로젝트 이름을 설정합니다.
3. `wandb sync YOUR_RUN_DIRECTORY`를 실행하여 메트릭을 저희 클라우드 서비스로 푸시하고 호스팅 된 웹 앱에서 결과를 확인합니다.

### **툴이 훈련 데이터를 추적하거나 저장하나요?**

 SHA 또는 고유 식별자\(identifier\)를 `wandb.config.update(...)`로 전달하여 데이터세트를 훈련 실행과 연결하실 수 있습니다. W&B는 로컬 파일 이름으로 `wandb.save`가 호출되지 않는 한 어떠한 데이터도 저장하지 않습니다.

### **시스템 메트릭을 얼마나 자주 수집되나요?**

 기본값으로 메트릭은 2초마다 수집되고 30초 동안 평균으로 계산됩니다. 더 높은 해상도의 메트릭이 필요하신 경우, contact@wandb.com으로 이메일을 보내주시기 바랍니다.

### **Python에서만 작동하나요?**

 현재 라이브러리는 Python 2.7+ & 3.6+ 프로젝트에서만 작동합니다. 위에서 언급된 아키텍처를 통해 저희는 다른 언어와 쉽게 통합할 수 있어야 합니다. 다른 언어를 모니터링해야 하는 경우, [contact@wandb.com](mailto:contact@wandb.com)으로 저희에게 메모를 남겨주시기 바랍니다.

### **코드나 데이터세트 예시 외에 메트릭만 로그할 수 있나요?**

 **데이터 세트 예시**

 기본값으로 저희는 여러분의 어떠한 데이터세트 예시도 로그 하지 않습니다. 웹 인터페이스에서 예시 예측을 확인하시려면 이 기능을 분명하게 설정하실 수 있습니다.

 **코드 로깅**

코드 로깅을 끄기 위한 두 가지 방법이 있습니다:

1.  모든 코드 로깅을 해제하려면 **WANDB\_DISABLE\_CODE을 true로 설정합니다. 저희는 SHA 또는 디프 패치\(diff patch\)을 찾지 않습니다.**
2. 서버로의 디프 패치\(diff patch\) 동기화를 해제하시려면 **WANDB\_IGNORE\_GLOBS**를 \*.patch로 설정합니다. 로컬에서는 계속 사용하실 수 있으며 [wandb restore](https://docs.wandb.com/library/cli#restore-the-state-of-your-code)명령으로 이를 적용할 수 있습니다. 

###  **로깅이 제 훈련을 차단하나요?**

“로깅 함수\(logging function\)는 게으른가요\(Lazy\)? 결과를 서버에 전송한 다음 로컬 운용\(local operations\)을 계속하기 위해 네트워크에 의존하고 싶지 않습니다.”

 **wandb.log** 호출은 로컬 파일에 라인을 작성합니다. 이는 어떠한 네트워크 요청도 차단하지 않습니다. wandb.init을 요청 시, 저희는 파일 시스템 변경에 주의를 기울이고 훈련 프로세스에서 비동기식으로 웹 서비스와 통신하는 새 프로세스를 동일 머신에서 시작합니다.  


###  **스무딩 알고리듬\(smoothing algorithm\)에는 어떤 공식\(formula\)을 사용하나요?**

저희는 TensorBoard와 같은 지수이동평균 공식\(exponential moving average formula\)를 사용합니다. 다음의 링크에서 설명을 참조하세요: [https://stackoverflow.com/questions/42281844/what-is-the-mathematics-behind-the-smoothing-parameter-in-tensorboards-scalar](https://stackoverflow.com/questions/42281844/what-is-the-mathematics-behind-the-smoothing-parameter-in-tensorboards-scalar).  


###  **W&B는 TensorBoard와 어떤 점이 다른가요?**

저희는 TensorBoard 사용자 여러분을 환영하며, [TensorBoard 통합](https://docs.wandb.ai/v/ko/integrations/tensorboard)을 지원합니다! 저희는 여러분 모두를 위한 실험 추적 툴 개선을 위해 최선을 다하고 있습니다. 저희 공동 창립자들이 W&B 작업을 시작했을 때, OpenAI에 실망한 TensorBoard 사용자들을 위한 툴을 개발하기 위해 영감을 받았습니다. 다음은 저희가 개선을 위해 중점을 두고 있는 몇 가지 사항입니다:

1. **모델 재현:** Weights & Biases는 실험 탐색 및 추후 모델 재현에 유용합니다. 저희는 메트릭뿐만 아니라 초매개변수 및 코드의 버전을 캡처하고, 프로젝트를 재현 할 수 있도록 모델체크포인트를 저장합니다.
2. **자동 구성:** 프로젝트를 공동작업자에게 넘기거나, 휴가를 낸 경우, W&B는 여러분이 시도한 모든 모델을 쉽게 확인할 수 있도록 해드립니다. 따라서 오래된 실험 재실행에 시간을 허비하지 않으셔도 됩니다.
3. **빠르고 유연한 통합:** 5분 안에 W&B를 여러분의 프로젝트에 추가하세요. 저희의 무료 오픈소스 Python 패키지를 설치하시고, 여러분의 코드에 몇 줄만 추가하세요. 그리고 나면 모델을 실행할 때마다, 훌륭한 로그 된 메트릭과 레코드를 가지게 됩니다.
4. **지속적인, 중앙 집중식 대시보드:** 로컬 머신, 랩 클러스터\(lab cluster\) 또는 클라우드의 스팟 인스턴스\(spot instances\) 등 모델을 훈련하는 모든 곳에서 동일한 중앙 집중식 대시보드를 제공합니다. 여러 머신에서 TensorBoard 파일을 복사 및 정리하는 데 시간을 허비할 필요가 없습니다.
5. **강력한 테이블:** 여러 모델에서의 결과를 검색, 필터, 정렬 및 그룹화합니다. 수 천개의 모델 버전을 살펴보고, 다양한 작업에 대한 최고 퍼포먼스 모델을 쉽게 찾으실 수 있습니다. TensorBoard는 대규모 프로젝트에서 잘 작동하도록 설계되어 있지는 않습니다.
6. **공동작업을 위한 툴:** W&B를 사용해서 복잡한 머신 러닝 프로젝트를 구성하세요. W&B로 링크를 쉽게 공유하실 수 있으며, 개인 팀\(private teams\)을 활용해 모두가 공유 프로젝트에 결과를 보내도록 할 수 있습니다. 또한 리포트\(reports\)를 통해 공동 작업을 지원합니다. 양방향 시각화\(interactive visualizations\)를 추가하고, 마크다운\(markdown\)에서 여러분의 작업 내용을 설명하세요. 작업 로그 보관, 상사와의 결과 공유 또는 여러분의 연구실에 결과를 제시할 수 있는 유용한 방법입니다.

[무료 개인 계정](http://app.wandb.ai/)으로 시작하세요 →​

### **훈련 코드에서 실행 이름을 어떻게 구성하나요?**

wandb.init을 호출할 때 훈련 스크립트 상단에 다음과 같은 실험 이름을 전달합니다: `wandb.init(name="my awesome run")`

###  **어떻게 스크립트에 임의의 실행 이름을 얻나요?**

`wandb.run.save()`을 호출하고 `wandb.run.name`을 통해 이름을 얻으십시오.

###  **아나콘다\(anaconda\) 패키지가 있나요?**

아나콘다 패키지는 없지만 다음을 사용해 wandb를 설치하실 수 있습니다:

```text
conda activate myenv
pip install wandb
```

설치에서 문제가 발생한 경우 저희에게 알려주시기 바랍니다. [패키지 관리에 관한 아나콘다 문서](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-pkgs.html)에는 몇 가지 유용한 안내사항이 있습니다.

###  **wandb가 터미널\(terminal\) 또는 jupyter notebook 출력에 작성하는 것을 중지하려면 어떻게 해야 하나요?**

환경변수 [WANDB\_SILENT](https://docs.wandb.com/library/environment-variables)를 설정하십시오.

 노트북에서 다음을 수행합니다:

```text
%env WANDB_SILENT true
```

Python 스크립트에서 다음을 수행합니다:

```text
os.environ["WANDB_SILENT"] = "true"
```

### **Wandb로 어떻게 작업을 종료\(kill\)하나요?**

Ctrl 키+D 키를 눌러 wandb로 계측\(instrument\)된 스크립트를 중지합니다.

###  **네트워크 문제를 해결하려면 어떻게 해야 하나요?**

1. SSL certificate\(인증서\)를 업그레이드합니다. Ubuntu 서버에서 스크립트를 실행하고 있는 경우, update-ca-certificates를 실행합니다. 보안 취약성 때문에 유효한 SSL 인증서가 없으면 저희는 훈련 로그를 동기화 할 수 없습니다.
2. 네트워크가 불안정한 경우, [오프라인 모드](https://docs.wandb.com/resources/technical-faq#can-i-run-wandb-offline)에서 훈련을 실행하고 인터넷에 액세스 할 수 있는 머신에서 파일을 저희에게 동기화합니다.
3.  머신에서 작동하며, 파일을 저희 클라우드 서버에 동기화 하지 않는 [W&B 로컬](https://docs.wandb.com/self-hosted/local)을 실행합니다.

**SSL CERTIFICATE\_VERIFY\_FAILED:** 이 오류는 여러분 회사의 방화벽 때문에 발생하는 것일 수 있습니다. 로컬 CA를 설정하고 다음을 사용하실 수 있습니다:

`export REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt`

###   **모델을 훈련하는 동안 인터넷 연결이 끊어지면 어떻게 되나요?**

저희 라이브러리가 인터넷에 연결할 수 없는 경우, 재시도 루프\(retry loop\)가 입력되며 네트워크가 복원될 때까지 메트릭 스트리밍을 계속해서 시도합니다. 이 시간 동안, 여러분의 프로그램은 계속해서 실행될 수 있습니다.

인터넷이 없는 머신에서 실행하는 경우, 메트릭만 하드드라이브에 로컬로 저장되도록 WANDB\_MODE=dryrun을 설정 하실 수 있습니다. 나중에 `wandb sync DIRECTORY`을 호출하여 데이터가 서버로 스트리밍 되도록 할 수 있습니다.

### **메트릭을 두 가지 다른 시간 척도\(time scale\)에 로그 할 수 있나요? \(예: 배치당 훈련 정확도\(training accuracy per batch\) 및 에포크당 검증 정확도\(validation accuracy per epoch\)을 로그하고 싶습니다.\)**

 네. 여러 메트릭을 로깅한 다음 x축 값을 설정하여 이 작업을 수행할 수 있습니다. 따라서 한 단계에서 `wandb.log({'train_accuracy': 0.9, 'batch': 200})`를 호출하고 다른 단계에서는 `wandb.log({'val_acuracy': 0.8, 'epoch': 4})`를 호출하실 수 있습니다.

###  **최종 평가 정확도\(evaluation accuracy\)와 같이 시간 경과에 따라 변경되지 않는 메트릭을 로그하려면 어떻게 해야 되나요?**

이 경우 wandb.log\({'final\_accuracy': 0.9}를 사용하실 수 있습니다. 기본값으로 wandb.log\({'final\_accuracy'}\)가 실행 테이블\(runs table\)에 표시되는 값인 wandb.settings\['final\_accuracy'\]을 업데이트 합니다.

###  **실행이 완료된 후 추가 메트릭을 기록하려면 어떻게 해야 되**

이 작업을 수행하기 위한 몇 가지 방법이 있습니다.

복잡한 워크플로우의 경우 여러 실행을 사용하고 [wandb.init](https://docs.wandb.ai/v/ko/library/init)의 그룹 매개변수를 단일 실험의 일부로 실행되는 모든 프로세스에 고유한 값에 설정하는 것을 권장합니다. [runs table\(실행 테이블\)](file:////app/pages/run-page)은 그룹 ID별로 테이블을 자동으로 그룹화 하며 시각화는 예상대로 작동하게 됩니다. 이러한 작업을 통해 개별 프로세스가 모든 결과를 단일 공간에 로그할 때 여러 실험 및 훈련 실행을 실행하실 수 있습니다.

 보다 간단한 워크플로의 경우, resume=True and id=UNIQUE\_ID wandb.init을 호출한 다음 동일한 id=UNIQUE\_ID으로 wandb.init를 호출하실 수 있습니다. 그러면 [wandb.log](file:////library/log) 또는 wandb.summary를 통해 정상적으로 로그할 수 있으며, 실행 값\(runs values\)이 업데이트됩니다.

어느 시점이든, 언제든지 [API](https://docs.wandb.com/ref/export-api)를 사용하여 추가적인 평가 메트릭을 추가하실 수 있습니다.

### **.log\(\) 과 .summary의 차이는 무엇인가요?**

요약\(summary\)는 로그가 나중에 플로팅\(plotting\)하기 위한 모든 값을 저장하는 동안 테이블에 표시되는 값입니다.

예를 들면, 정확도가 변경될 때 마다, `wandb.long`를 호출하는 것이 좋을 수도 있습니다. 일반적으로 .log만 사용하시면 됩니다. 또한 해당 메트릭에 대한 요약을 설정하지 않은 한 기본적으로 `wandb.log()`는 요약 값을 업데이트합니다.

 산점도 및 평행좌표그래프 또한 요약 값을 사용합니다. 반면 라인 플롯은 .log에 의해 설정된 모든 값을 플롯\(plot\)합니다.

저희가 두 가지 모두를 사용하는 이유는 일부 사용자들이 로그된 마지막 정확도 대신, 예를 들어, 최적 정확도\(optimal accuracy\)를 요약에 반영하기를 원해서, 요약을 수동으로 설정하는 것을 선호하기 때문입니다.

### **gcc가 없는 환경에서 wandb Python 라이브러리를 설치하려면 어떻게 해야 되나요?**

 `wandb` 설치 시도 중 다음의 오류가 나타나는 경우:

```text
unable to execute 'gcc': No such file or directory
error: command 'gcc' failed with exit status 1
```

 사전 빌드된 휠\(pre-built wheel\)에서 직접 psutil을 설치할 수 있습니다. Python 버전 밑 OS를 다음에서 찾으실 수 있습니다: [https://pywharf.github.io/pywharf-pkg-repo/psutil](https://pywharf.github.io/pywharf-pkg-repo/psutil)  


 예를 들어, Linux의 Python 3.8에 psutil을 설치하려면 다음을 수행합니다:

```text
pip install https://github.com/pywharf/pywharf-pkg-repo/releases/download/psutil-5.7.0-cp38-cp38-manylinux2010_x86_64.whl/psutil-5.7.0-cp38-cp38-manylinux2010_x86_64.whl#sha256=adc36dabdff0b9a4c84821ef5ce45848f30b8a01a1d5806316e068b5fd669c6d
```

psutil이 설치된 다음, `pip install wandb`로 wandb를 설치하실 수 있습니다.

  


