# Code Saving

 기본값으로, 저희는 최선 git 커밋 해시만을 저장합니다. 더 많은 코드 기능을 사용하여 UI에서 실험 간 코드를 동적으로 비교하실 수 있습니다.

 `wandb` 버전0 0.8.28부터, 저희는 `wandb.init()`을 호출하는 주 훈련 파일\(main training file\)에서 코드를 저장할 수 있습니다. 이렇게 하면, 대시보드에 동기화되어 실행 페이지의 탭과 Code Comparer 패널에 표시됩니다. [설정 페이지\(settings page\)](https://app.wandb.ai/settings)로 가셔서 기본값으로 코드 저장을 활성화 하십시오.

![Here&apos;s what your account settings look like. You can save code by default.](../../../.gitbook/assets/screen-shot-2020-05-12-at-12.28.40-pm.png)

##  **Code Comparer\(코드 비교기\)**

작업공간 또는 리포트의 **+** 버튼을 클릭하여 새 패널을 추가하고, Code Comparer를 선택하세요. 프로젝트에서 두 개의 실험을 디핑\(diff\)하고 코드의 어떤 행이 변경되었는지 확인하십시오. 다음은 그 예시입니다:

![](../../../.gitbook/assets/cc1.png)

##  **Jupyter Session History\(Jupyter 세션 히스토리\)**

**wandb** 버전 0.8.34부터 저희 라이브러리는 Jupyter 세션을 저장합니다. Jupyter내에서 **wandb.init\(\)**을 호출하면, 저희는 훅\(hook\)을 추가하여 자동으로 여러분의 현재 세션에서 수행된 코드 히스토리를 포함한 Jupyter notebook을 저장합니다. 이 세션 히스토리는 코드 디렉토리 아래의 실행 파일 브라우저\(runs file browser\)에서 찾으실 수 있습니다:

![](../../../.gitbook/assets/cc2%20%284%29%20%284%29.png)

 이 파일을 클릭하시면 iPython’s display method를 호출하여 생성된 출력과 함께 세션에서 수행된 셀이 표시됩니다. 이를 통해 정확하게 어떤 코드가 주어진 실행에서 Jupyter내에서 실행되었는지 확인하실 수 있습니다. 가능한 경우, 저희는 코드 디렉토리에서도 찾으실 수 있는 가장 최신 버전의 notebook 또한 저장합니다.

![](../../../.gitbook/assets/cc3%20%283%29%20%281%29.png)

##  **Jupyter diffing\(Jupyter 디핑\)**

마지막 보너스 기능 중 하나는 notebooks를 디핑하는 기능입니다. Code Comparer 패널에 raw JSON을 표시하는 것 대신, 각 셀을 추출하여 변경된 모든 라인을 표시합니다. Jupyter를 저희 플랫폼에 더 깊숙이 통합하기 위한 몇 가지 기능이 계획되어 있습니다.

