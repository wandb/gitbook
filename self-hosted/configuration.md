---
description: Comment configurer l’installation du Serveur W&B Local
---

# Advanced Configuration

 Votre serveur W&B Local est prêt à être utilisé au démarrage. Néanmoins, de nombreuses autres options de configurations avancées sont disponibles sur la page `/system-admin` de votre serveur, une fois qu’il est installé et lancé. Vous pouvez nous envoyer un e-mail à l’adresse [contact@wandb.com](mailto:contact@wandb.com) pour demander un essai gratuit d’une licence d’accès pour plus d’utilisateurs et d’équipes.

## **Configuration par codage**

Tous les paramètres de configuration peuvent être paramétrés via l’interface utilisateur, cependant, si vous préférez les gérer via le codage, vous pouvez configurer les variables d’environnement suivantes :

| **Variable d’environnement** | Description |
| :--- | :--- |
| LICENSE | Votre licence wandb/local |
| MYSQL | La chaîne de connexion MySQL |
| BUCKET | Le compartiment \(bucket\) S3 / GCS pour le stockage les données |
| BUCKET\_QUEUE | La file d’attente SQS / Google PubSub pour les évènements de création d’objet |
| NOTIFICATIONS\_QUEUE | La file d’attente SQS pour la publication des événements d’exécution |
| AWS\_REGION | La région AWS de votre compartiment \(bucket\) |
| HOST | Le FQD de votre instance, c-à-d[https://my.domain.net](https://my.domain.net/)​ |
| AUTH0\_DOMAIN | Le domaine Auth0 de votre locataire \(tenant\) |
| AUTH0\_CLIENT\_ID | L’ID Client Auth0 d’application |
| SLACK\_CLIENT\_ID | L’ID client de l’application Slack quevous voulez l’utiliser pour les alertes |
| SLACK\_SECRET | L’identification secrète de l’application Slack que vous voulez utiliser pour les alertes |

## Authentication

Par défaut, un serveur W&B Local s’exécute avec une gestion d’utilisateurs manuelle, qui permet d’avoir jusqu’à 4 utilisateurs. Les versions avec licence de wandb/local déverrouillent également une authentification unique \(SSO\) en utilisant Auth0.

Votre serveur prend en charge tout fournisseur d’authentification qui est pris en charge par [Auth0](https://auth0.com/). Vous devriez configurer votre propre domaine et application Auth0 qui seront contrôlés par vos équipes.

Après avoir créé l’application Auth0, il vous faudra configurer les fonctions de rappel \(callbacks\) Auth0 surl’hôte de votre serveur W&B. Par défaut, le serveur prend en charge l’http de l’adresse IP publique ou privée qui est fournie par l’hôte. Vous pouvez aussi configurer un nom d’hôte DNS et un certificat SSL, si vous le souhaitez.

* Configurez l’URL de la fonction de rappel \(callback\) sur `http(s)://YOUR-W&B-SERVER-HOST`
* Configurez l’origine Web autorisée \(Allowed Web Origin\) sur `http(s)://YOUR-W&B-SERVER-HOST`
* Configurez l’URL de déconnexion \(Logout\) sur `http(s)://YOUR-W&B-SERVER-HOST/logout`

![Param&#xE8;tres Auth0](../.gitbook/assets/auth0-1.png)

Sauvegardez l’ID Client et le domaine depuis votre app Auth0.

![Param&#xE8;tres Auth0](../.gitbook/assets/auth0-2.png)

Puis, naviguez jusqu’à la page de paramètres W&B, `http(s)://YOUR-W&B-SERVER-HOST/admin-settings`. Activez l’option "Personnaliser l’Authentification avec Auth0" \(Customize Authentication with Auth0\), et remplissez l’ID client et le domaine de votre app Auth0.

![Param&#xE8;tres d&#x2019;authentification d&#x2019;entreprise](../.gitbook/assets/enterprise-auth.png)

Ensuite, naviguez jusqu’à la page de paramètres de W&B, http\(s\)://YOUR-W&B-SERVER-HOST/admin-settings. Activez l’option Personnaliser l’authentification avec Auth0 \(Customize Authentication with Auth0\), et remplissez l’ID client et le domaine de votre app Auth0.

##  Stockage de fichiers

Par défaut, un serveur Entreprise W&B sauvegarde les fichiers sur un disque de données local avec une capacité que vous déterminez lorsque vous donnez les informations de votre instance. Pour prendre en charge le stockage illimité de fichiers, vous pouvez configurer votre serveur pour utiliser un compartiment \(bucket\) de stockage de fichiers externe en cloud avec une API compatible avec S3.

### Amazon Web Services

Pour utiliser un compartiment AWS S3 en tant que backend de stockage de fichiers pour W&B, il vous faudra créer un compartiment, ainsi qu’une file d’attente \(queue\) SQS configurée pour recevoir les notifications de création d’objet depuis ce compartiment. Votre instance aura besoin d’autorisations pour lire depuis cette file d’attente.

**Créer une file d’attente SQS**

 Tout d’abord, créez une file d’attente standard SQS. Ajoutez une autorisation pour toutes les fonctions principales pour les actions `SendMessage` et `ReceiveMessage` ainsi que `GetQueueUrl`. \(Si vous le souhaitez, vous pouvez la verrouiller en utilisant un document de politique avancé.\)

![Param&#xE8;tres de stockage de fichiers d&#x2019;entreprise](../.gitbook/assets/sqs-perms.png)

 **Créer un compartiment S3 et des notifications de compartiment**

 Ensuite, créez un compartiment S3. Sous la page de propriétés du compartiment dans la console, dans la section Événements \(Events\) des Paramètres Avancés \(Advanced Settings\), cliquez sur Ajouter une notification \(Add notification\), et configurez tous les évènements de création d’objet qui doivent être envoyés à la file d’attente SQS que vous avez configurée un peu plus tôt.

![Param&#xE8;tres de stockage de fichiers d&#x2019;entreprise](../.gitbook/assets/s3-notification.png)

Permettre l’accès CORS : votre configuration CORS devrait ressembler à ceci :

Activer l’accès CORS \(partage de ressources entre origines multiples\) – votre configuration devrait ressembler à ceci :

```markup
<?xml version="1.0" encoding="UTF-8"?>
<CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
<CORSRule>
    <AllowedOrigin>http://YOUR-W&B-SERVER-IP</AllowedOrigin>
    <AllowedMethod>GET</AllowedMethod>
    <AllowedMethod>PUT</AllowedMethod>
    <AllowedHeader>*</AllowedHeader>
</CORSRule>
</CORSConfiguration>
```

**Configurer le Serveur W&B**

Pour finir, allez sur la page des paramètres de W&B, `http(s)://YOUR-W&B-SERVER-HOST/admin-settings`. Activez l’option Utiliser un backend de stockage de fichiers externe \(Use an external file storage backend\), et indiquez le compartiment s3, la région et la file d’attente SQS dans le format suivant :

* **Bucket de stockage de fichiers**: `s3://<bucket-name>`
* **Région de stockage de fichiers**: `<region>`
*  **Abonnement aux notifications**: `sqs://<queue-name>`

![Param&#xE8;tres de stockage de fichiers AWS](../.gitbook/assets/aws-filestore.png)

 Cliquez sur Mettre les paramètres à jour et redémarrer W&B \(Update settings and restart W&B\) pour appliquer les nouveaux paramètres.

###  Google Cloud Platform

Pour utiliser un compartiment de stockage GCP en tant que backend de stockage de fichiers pour W&B, vous aurez besoin de créer un compartiment, ainsi qu’un sujet \(topic\) et un abonnement pubsub \(publish-subscribe\)configurés pour recevoir les messages de création d’objet depuis ce compartiment.

**Créer un sujet et un abonnement Pubsub**

Allez à Pub/Sub&gt;Sujets dans la console GCP, et cliquez sur Créer un sujet. Choisissez un nom et créez un sujet.

Ensuite, cliquez sur Créer un abonnement dans le tableau d’abonnements, en bas de la page. Choisissez un nom et assurez-vous que le Type de distribution soit paramétré sur Pull. Puis, cliquez sur Créer.

Assurez-vous que le compte de service ou le compte sous lequel votre instance s’exécute ait accès à cet abonnement.

**Créer un compartiment de stockage**

Allez à Stockage&gt;Navigateur dans la Console GCP, et cliquez sur Créer un compartiment \(Create bucket\). Assurez-vous de choisir Standard pour la classe de stockage \(storage class\).

Assurez-vous que le compte de service ou le compte sous lequel votre instance s’exécute ait accès à cet abonnement.

**Créer une notification pubsub**

La création d’un fil de notification depuis le compartiment de stockage vers le sujet pubsub ne peut malheureusement être réalisée que dans la console. Assurez-vous d’avoir installé gsutil et d’être connecté au bon projet GCP, puis lancez ce qui suit :​

```bash
gcloud pubsub topics list  # list names of topics for reference
gsutil ls                  # list names of buckets for reference

# create bucket notification
gsutil notification create -t <TOPIC-NAME> -f json gs://<BUCKET-NAME>
```

​[Plus de références sont disponibles sur le site web du stockage cloud.](https://cloud.google.com/storage/docs/reporting-changes)​

**Ajouter des autorisations signées**

Pour la création d’URL signées de fichiers, votre instance W&B devra également avoir l’autorisation iam.serviceAccounts.signBlob sur GCP. Vous pouvez l’ajouter en ajoutant le rôle de créateur de jetons du compte de service \(Service Account Token Creator\) au compte de service ou au membre IAM sous lequel votre instance s’exécute.

**Accorder des autorisations au nœud \(node\) qui exécute W&B**

Le nœud sur lequel W&B Local s’exécute doit être configuré pour permettre l’accès au s3 et au SQS. En fonction du type de déploiement du serveur pour lequel vous avez opté, il se pourrait que vous devriez ajouter les déclarations de politique \(policy statements\) suivantes au rôle de votre nœud :

```text
{
   "Statement":[
      {
         "Sid":"",
         "Effect":"Allow",
         "Action":"s3:*",
         "Resource":"arn:aws:s3:::<WANDB_BUCKET>"
      },
      {
         "Sid":"",
         "Effect":"Allow",
         "Action":[
            "sqs:*"
         ],
         "Resource":"arn:aws:sqs:<REGION>:<ACCOUNT>:<WANDB_QUEUE>"
      }
   ]
}
```

 **Configurer le serveur W&B**

 Pour finir, allez à la page des paramètres W&B, `http(s)://YOUR-W&B-SERVER-HOST/admin-settings`. Activez l’option Utiliser un backend de stockage de fichiers externe \(Use an external file storage backend\), et indiquez le compartiment s3, la région et la file d’attente SQS dans le format suivant :

*  **Compartiment de stockage de fichiers**: : `gs://<bucket-name>`
* **Région de stockage de fichiers**: blank
* **Abonnement aux notifications**: `pubsub:/<project-name>/<topic-name>/<subscription-name>`

![Param&#xE8;tres de stockage de fichiers GCP](../.gitbook/assets/gcloud-filestore.png)

  Cliquez sur Mettre les paramètres à jour et redémarrer W&B \(Update settings and restart W&B\) pour appliquer les nouveaux paramètres.

### Azure

qu’espace de stockage de fichiers pour W&B, il vous faudra créer un compte de stockage \(si vous n’en avez pas déjà un que vous souhaitez utiliser\), créer un conteneur blob et une file d’attente \(queue\) dans ce compte de stockage, puis créer un événement d’inscription qui envoie des notifications Blob créé \(blob created\) à la file d’attente depuis le conteneur blob.

####  **Créer un compte de stockage**

Si vous avez déjà un compte de stockage que vous souhaitez utiliser, vous pouvez sauter cette étape.

Allez à [Comptes de stockage &gt; Ajouter](https://portal.azure.com/#create/Microsoft.StorageAccount) \(Storage Accounts &gt; Add\) dans le portail Azure. Sélectionnez un abonnement Azure, puis sélectionnez un groupe de ressources ou créez-en un nouveau. Entrez un nom pour votre compte de stockage.

![Mise en place du compte de stockage Azure](../.gitbook/assets/image%20%28106%29.png)

 Configuration du compte de stockage Azure

![V&#xE9;rification des d&#xE9;tails de compte de stockage Azure](../.gitbook/assets/image%20%28114%29.png)

#### **Créer le conteneur blob**

Allez à [Comptes de Stockage](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Storage%2FStorageAccounts) sur le portail Azure et cliquez sur votre nouveau compte de stockage. Dans le tableau de bord du compte de stockage, cliquez sur Service Blob &gt; Conteneurs \(Blob service &gt; Containers\)dans le menu :

![](../.gitbook/assets/image%20%28102%29.png)

 Créez un nouveau conteneur, et paramétrez-le sur Privé \(Private\) :

![](../.gitbook/assets/image%20%28110%29.png)

  
Allez dans Paramètres&gt;CORS&gt;Service Blob, et entrez l’IP de votre serveur wandb en tant qu’origine autorisée \(allowed origin\), avec les méthodes autorisées `GET` et `PUT`, et tous les en-têtes autorisés et exposés, puis enregistrez vos paramètres CORS.

![](../.gitbook/assets/image%20%28119%29.png)

#### **Créer la File d’attente**

Allez à Service de file d’attente&gt;Files d’attente \(Queue Service &gt; Queues\) dans votre compte de stockage, et créez une nouvelle file d’attente :

![](../.gitbook/assets/image%20%28101%29.png)

 Allez dans Événements \(Events\) dans votre compte de stockage, et créez un abonnement à un événement :

![](../.gitbook/assets/image%20%28108%29.png)

Attribuez à l’abonnement aux événements le Schéma de la grille d’événements "Event Grid Schema", filtrez pour n’avoir que le type d’événement Blob créé \(Blob Created\), paramétrez le Type de terminal \(Endpoint Type\) sur Files d’attente de Stockage \(Storage Queues\), puis sélectionnez le compte de stockage/la file d’attente comme terminal.

![](../.gitbook/assets/image%20%28116%29.png)

Dans l’onglet Filtres, activez le filtrage de sujet pour les sujets qui commencent avec`/blobServices/default/containers/your-blob-container-name/blobs/`

![](../.gitbook/assets/image%20%28105%29.png)

#### **Configurer le serveur W&B**

Allez dans Paramètres &gt; Afficher les clefs \(Settings &gt; Access keys\) dans votre compte de stockage, cliquez sur Affichez les clefs \(Show keys\), puis copiez soit key1 &gt; Key, soit key2 &gt; Key. Paramétrez cette clef sur votre serveur W&B en tant que variable d’environnement `AZURE_STORAGE_KEY`.

![](../.gitbook/assets/image%20%28115%29.png)

Enfin, naviguez sur la page de paramètres W&B, `http(s)://YOUR-W&B-SERVER-HOST/admin-settings`. Activez l’option "Utiliser un backend de stockage de fichiers externe" \(Use an external file storage backend\), et indiquez le bucket s3, la région, et la file d’attente SQS dans le format suivant :

*  **Bucket de Stockage de Fichiers** : `az://<storage-account-name>/<blob-container-name>`
*  **Abonnement Notifications**: `az://<storage-account-name>/<queue-name>`

![](../.gitbook/assets/image%20%28109%29.png)

 Cliquez sur "Mettre les paramètres à jour " \(Update settings\) pour appliquer les nouveaux paramètres.

## Slack

Pour intégrer votre installation locale W&B avec Slack, il vous faudra créer une application Slack convenable.

#### **Créer l’application Slack**

Rendez-vous sur [https://api.slack.com/apps](https://api.slack.com/apps) et sélectionnez **Créez Nouvelle App** \(Create New App\) en haut à droite.

![](../.gitbook/assets/image%20%28123%29.png)

 Vous pouvez lui donner le nom que vous souhaitez, mais ce qui est important, c’est de sélectionner le même workspace Slack que celui que vous voulez utiliser pour les alertes.

![](../.gitbook/assets/image%20%28124%29.png)

####  **Configurer l’application Slack**

Maintenant que l’application Slack est prête, il faut que nous autorisions l’utilisation d’un bot OAuth. Sélectionnez **OAuth & Permissions** dans le panneau latéral gauche.

![](../.gitbook/assets/image%20%28125%29.png)

Sous **Scopes**, fournissez au bot le scope **incoming\_webhook.**

![](../.gitbook/assets/image%20%28128%29%20%281%29%20%281%29.png)

Pour finir, configurez **l’URL de Redirection \(Redirect URL\)** pour la faire pointer vers votre installation W&B. Vous devriez utiliser la même valeur que celle paramétrée pour votre **Hôte frontend** \(Frontend Host\) dans vos paramètres de système locaux. Vous pouvez spécifier plusieurs URL si vous avez différents mappages DNSdans votre instance.

![](../.gitbook/assets/image%20%28127%29.png)

Cliquez sur **Sauvegarder les URLs** \(Save URLs\) une fois que vous avez terminé.

Pour sécuriser davantage votre application Slack et évitez les abus, vous pouvez spécifier une plage d’IP sous **Restriction d’usage de jeton API \(Restrick API Token Usage\)** pour définir une liste blanche de l’IP ou la plage d’IP de votre ou vos instances W&B.

**Enregistrer votre application Slack avec W&B**

Allez sur la page des **Paramètres système \(System Settings\)** de votre instance W&B. Cochez la case pour activer une application Slack personnalisée :

![](../.gitbook/assets/image%20%28126%29.png)

Il vous faudra fournir l’ID client de votre application Slack et son identification secrète, que vous pouvez trouver dans l’onglet **Informations de base \(Basic Information\)**.

![](../.gitbook/assets/image%20%28120%29.png)

Et voilà ! Vous pouvez maintenant vérifier que tout fonctionne en mettant en place une intégration Slack dans l’application W&B. Visitez [cette page ](https://docs.wandb.ai/v/fr/app/features/alerts)pour de plus amples informations.

