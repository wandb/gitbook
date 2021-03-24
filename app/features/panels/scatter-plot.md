# Scatter Plot

산점도를 사용하여 여러 실행을 비교하고 실험이 어떻게 수행되는지 시각화하실 수 있습니다. 다음은 저희가 추가한 몇 가지 사용자 지정 기능입니다:

1. min\(최소\), max\(최대\), average\(평균\)를 따라 선 그리기
2. 사용자 정의 메타데이터 툴팁\(tooltips\)
3. 포인트 색상 제어
4. 축 범위 설정
5. 축을 로그 스케일로 전환

 다음은 몇 주간의 실험에서 여러 모델의 검증 정확도에 관한 예시입니다. 툴팁\(tooltip\)은 배치 사이즈\(batch size\) 및 드롭아웃\(dropout\)뿐만 아니라 축의 값을 포함하도록 사용자 지정되었습니다. 검증 정확도의 이동 평균을 작성한 선\(line\)도 있습니다.  
  
 [라이브 예시 보기 →](https://app.wandb.ai/l2k2/l2k/reports?view=carey%2FScatter%20Plot)​

![](https://paper-attachments.dropbox.com/s_9D642C56E99751C2C061E55EAAB63359266180D2F6A31D97691B25896D2271FC_1579031258748_image.png)

##  **공통 질문**

단계별 플로팅\(plot\) 대신 메트릭의 최대값을 플로팅\(plot\)하는 것이 가능한가요?

가장 좋은 방법은 메트릭의 산점도\(Scatter Plot\)를 생성하고, Edit\(편집\) 메뉴로 이동한 뒤, Annotations\(주석\)를 선택하는 것입니다. 여기서 값의 이동 최대\(running max\)를 작성\(plot\)하실 수 있습니다.  


