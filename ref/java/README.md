---
description: Java clinet 라이브러리 개요
---

# Java Library \[Beta\]

Python 라이브러리와 비슷하게, 머신러닝 모델 조직 및 실험 추적을 위해 Java client를 제공합니다.

Java Client의 소스 코드는 [Github repository](https://github.com/wandb/client-ng-java)에서 찾으실 수 있습니다.

{% page-ref page="wandbrun-builder.md" %}

{% page-ref page="wandbrun.md" %}

###  **설치**

1. 최신 버전의 wandb Python client를 설치하세요: `pip install wandb[grpc] –upgrade`
2. Wandb jar 파일을 Java 프로젝에 포함하시면 됩니다.

   **Maven**: pom.xml 파일에 jar 파일을 추가 하여 포함하실 수 있습니다. maven repository 사용:

   ```markup
   <dependency>
       <groupId>com.wandb.client</groupId>
       <artifactId>client-ng-java</artifactId>
       <version>1.0-SNAPSHOT</version>
   </dependency>
   ```

   또는 [Github Package](https://github.com/wandb/client-ng-java/packages/381057)에서 직접 jar 파일을 다운로드할 수 있습니다:

   ```markup
   <dependencies>
       <dependency>
           <groupId>com.wandb.client</groupId>
           <artifactId>client-ng-java</artifactId>
           <version>1.0-SNAPSHOT</version>
           <scope>system</scope>
           <systemPath>/root/path/to/jar/file.jar</systemPath>
       </dependency>
   </dependencies>
   ```

