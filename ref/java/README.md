---
description: Aperçu de notre librairie de client Java
---

# Java Library \[Beta\]

Similaire à notre librairie Python, nous proposons un client Java pour instrumenter votre modèle d’apprentissage automatique et garder une trace de vos expériences. Cette librairie est constituée de deux classes simples qui sont utilisées comme wrapper autour de la Librairie Python.

Vous pouvez trouver le code source pour le Client Java dans le [répertoire Github](https://github.com/wandb/client-ng-java).  


{% page-ref page="wandbrun-builder.md" %}

{% page-ref page="wandbrun.md" %}

### Installation

1. Installer la dernière version de notre client Python wandb `pip install wandb[grpc] --upgrade`
2. Inclure simplement le fichier jar Wandb dans votre Projet Java.

   **Maven** : peut être inclus en ajoutant votre fichier jar à vote fichier `pom.xml.`

               En utilisant répertoire maven :

   ```markup
   <dependency>
       <groupId>com.wandb.client</groupId>
       <artifactId>client-ng-java</artifactId>
       <version>1.0-SNAPSHOT</version>
   </dependency>
   ```

   Ou vous pouvez télécharger le fichier jar directement depuis le Package [Github](https://github.com/wandb/client-ng-java/packages/381057) :

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

