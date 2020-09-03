---
description: Overview of our Java client library
---

# Java Libary \[Beta\]

Similar to our python library, we offer a Java client to instrument your machine learning model and track experiments. 

This library consists of two simple classes that are used as a wrapper around the Python Library.

{% page-ref page="wandbrun.builder.md" %}

{% page-ref page="wandbrun.md" %}

### Installation

1. Install the latest RC version of the wandb python client.
2. Simply include the Wandb jar file in your Java Project.
   1. **Maven**: this can be included by adding the jar file to your `pom.xml`  file.  
      Using GitHub repository:

      ```markup
      <dependency>
          <groupId>com.wandb.client</groupId>
          <artifactId>client-ng-java</artifactId>
          <version>1.0-SNAPSHOT</version>
      </dependency>
      ```

      Or using the jar file directly:

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

xm

