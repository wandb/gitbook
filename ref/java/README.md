---
description: Javaクライアントライブラリの概要
---

# Java Library \[Beta\]

Pythonライブラリと同様に、機械学習モデルを計測して実験を追跡するためのJavaクライアントを提供しています。このライブラリは、Pythonライブラリのラッパーとして使用される2つの単純なクラスで構成されています。

Javaクライアントのソースコードは[Githubリポジトリ](https://github.com/wandb/client-java)にあります。

{% page-ref page="wandbrun-builder.md" %}

{% page-ref page="wandbrun.md" %}

###  インストール

1. **最新バージョンのwandb Pythonクライアントをインストールします。`pip install wandb[grpc] --upgrade`**  
2. **JavaプロジェクトにWandb jarファイルを含めるだけです**
3. **Maven：これはjarファイルをpom.xmlファイルに追加することで含めることができます。Mavenリポジトリの使用：**

**または、jarファイルをGithubパッケージから直接ダウンロードすることもできます。**

1. ```markup
   <dependency>
       <groupId>com.wandb.client</groupId>
       <artifactId>client-ng-java</artifactId>
       <version>1.0-SNAPSHOT</version>
   </dependency>
   ```



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

