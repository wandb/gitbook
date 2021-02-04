---
description: Resumen de nuestra biblioteca cliente de Java
---

# Java Library \[Beta\]

Tal como nuestra biblioteca de Python, ofrecemos un cliente en Java que instrumenta a tu modelo de aprendizaje de máquinas y hace el seguimiento de los experimentos. Esta biblioteca consiste de dos clases simples que son utilizadas como un wrapper de la Biblioteca de Python.

Puedes encontrar el código fuente para el Cliente de Java en el [repositorio de Github](https://github.com/wandb/client-ng-java).

{% page-ref page="wandbrun-builder.md" %}

{% page-ref page="wandbrun.md" %}

### Instalación

1. Instala la última versión del cliente de Python de wandb: `pip install wandb[grpc] –upgrade`
2. Simplemente incluye el archivo jar de Wandb en tu Proyecto Java.

   Maven: esto puede ser incluido al agregar el archivo jar a tu archivo `pom.xml`.

   Utilizando el repositorio de maven:

3. ```markup
   <dependency>
       <groupId>com.wandb.client</groupId>
       <artifactId>client-ng-java</artifactId>
       <version>1.0-SNAPSHOT</version>
   </dependency>
   ```

   O puedes descargar el archivo jar directamente desde el [Paquete de Github](https://github.com/wandb/client-ng-java/packages/381057):

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

