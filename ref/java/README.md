---
description: Overview of our Java client library
---

# Java Library \[Beta\]

Similar to our Python library, we offer a Java client to instrument your machine learning model and track experiments. This library consists of two simple classes that are used as a wrapper around the Python Library.

You can find the source code for the Java Client in the [Github repository](https://github.com/wandb/client-ng-java).

{% page-ref page="wandbrun-builder.md" %}

{% page-ref page="wandbrun.md" %}

### Installation

1. Install the latest version of the wandb Python client: `pip install wandb[grpc] --upgrade`
2. Simply include the Wandb jar file in your Java Project.

   **Maven**: this can be included by adding the jar file to your `pom.xml`  file.  
   Using maven repository:

   ```markup
   <dependency>
       <groupId>com.wandb.client</groupId>
       <artifactId>client-ng-java</artifactId>
       <version>1.0-SNAPSHOT</version>
   </dependency>
   ```

   Or you can download the jar file directly from the [Github Package](https://github.com/wandb/client-ng-java/packages/381057):

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

