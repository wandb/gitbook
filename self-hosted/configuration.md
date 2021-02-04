---
description: Comment configurer l’installation du Serveur W&B Local
---

# Advanced Configuration

Votre Serveur W&B Local est prêt à être utilisé dès le boot. Néanmoins, de nombreuses autres options de configurations avancées sont possibles, sur la page `/system-admin` de votre serveur, une fois qu’il est en place et lancé. Vous pouvez envoyer un email à [contact@wandb.com](mailto:contact@wandb.com) pour faire une requête de licence d’essai pour avoir plus d’utilisateurs et d’équipes.

## Configuration par le code

Tous les paramètres de configuration peuvent être paramétrés par l’IU, cependant, si vous préférez les gérer par du code, vous pouvez mettre en place les variables d’environnement suivantes :

| **Variable d’environnement** | Description |
| :--- | :--- |
| LICENSE | Votre licence wandb/local |
| MYSQL | La chaîne de connexion MySQL |
| BUCKET | Le bucket S3 / GCS pour stocker les données |
| BUCKET\_QUEUE | La file d’attente SQS / Google PubSub pour les évènements de création d’objet |
| NOTIFICATIONS\_QUEUE | La file d’attente SQS sur laquelle publier les événements de run |
| AWS\_REGION | La AWS Region où votre bucket vit |
| HOST | Le FQD de votre instance, i.e. [https://my.domain.net](https://my.domain.net/) |
| AUTH0\_DOMAIN | Le domaine Auth0 de votre locataire \(tenant\) |
| AUTH0\_CLIENT\_ID | L’ID Client Auth0 d’application |
| SLACK\_CLIENT\_ID | L’ID client de l’application Slack si vous voulez l’utiliser pour des alertes |
| SLACK\_SECRET | Le secret de l’application Slack que vous voulez utiliser pour les alertes |

## Authentication

Par défaut, un Serveur W&B Local s’exécute avec une gestion d’utilisateurs manuelle, qui permet d’avoir jusqu’à 4 utilisateurs. Les versions avec licence de wandb/local débloquent également SSO en utilisant Auth0.Votre serveur prend en charge tout fournisseur d’authentification qui est pris en charge par [Auth0](https://auth0.com/). Vous devriez mettre en place votre propre domaine et application Auth0 qui sera sous le contrôle de vos équipes.Après avoir créé l’app Auth0, il vous faudra configurer les callbacks Auth0 à l’hôte de votre Serveur W&B. Par défaut, le serveur prend en charge l’http de l’adresse IP publique ou privée qui est fournie par l’hôte. Vous pouvez aussi configurer un nom d’hôte DNS et un certificat SSL si vous le souhaitez.

* Réglez l’URL de Callback sur `http(s)://YOUR-W&B-SERVER-HOST`
* Réglez l’Origine Web Permise \(Allowed Web Origin\) sur `http(s)://YOUR-W&B-SERVER-HOST`
* Réglez l’URL de déconnection \(Logout\) sur `http(s)://YOUR-W&B-SERVER-HOST/logout`

![Param&#xE8;tres Auth0](../.gitbook/assets/auth0-1.png)

Sauvegardez l’ID Client et le domaine depuis votre app Auth0.

![Param&#xE8;tres Auth0](../.gitbook/assets/auth0-2.png)

Puis, naviguez jusqu’à la page de paramètres W&B, `http(s)://YOUR-W&B-SERVER-HOST/admin-settings`. Activez l’option "Personnaliser l’Authentification avec Auth0" \(Customize Authentication with Auth0\), et remplissez l’ID client et le domaine de votre app Auth0.

![Param&#xE8;tres d&#x2019;authentification d&#x2019;entreprise](../.gitbook/assets/enterprise-auth.png)

Enfin, cliquez sur "Mettre les paramètres à jour et redémarrer W&B" \(Update settings and restart W&B\).

##  Stockage de fichiers

Par défaut, un Serveur d’Entreprise W&B sauvegarde les fichiers sur un disque de données local avec une capacité que vous déterminez lorsque vous donnez les informations de votre instance. Pour prendre en charge le stockage illimité de fichiers, vous pouvez configurer votre serveur pour utiliser un bucket de stockage de fichier cloud externe avec une API compatible avec S3.

### Amazon Web Services

Pour utiliser un bucket AWS S3 comme backend de stockage de fichiers pour W&B, il vous faudra créer un bucket, ainsi qu’une file d’attente SQS \(queue\) configurée pour recevoir les notifications de création d’objet depuis ce bucket. Votre instance aura besoin des permissions pour lire depuis cette file d’attente.

**Créer une file d’attente SQS**

 Tout d’abord, créez une File d’attente Standard SQS. Ajoutez une permission pour tous les principaux pour les actions `SendMessage` et `ReceiveMessage` ainsi que `GetQueueUr`l. \(Si vous le souhaitez, vous pouvez renforcer le contrôle en utilisant un Policy Document avancé.\)

![Param&#xE8;tres de stockage de fichiers d&#x2019;entreprise](../.gitbook/assets/sqs-perms.png)

 **Créer un Bucket S3 et des Notifications de Bucket**

 Puis, créez un bucket S3. Sous la page de propriétés du bucket dans la console, dans la section "Événements" \(Events\) de "Paramètres Avancés" \(Advanced Settings\), cliquez sur "Ajouter notification" \(Add notification\), et configurez tous les évènements de création d’objet qui doivent être envoyés à la File d’attente SQS que vous avez configurée un peu plus tôt.

![Param&#xE8;tres de stockage de fichiers d&#x2019;entreprise](../.gitbook/assets/s3-notification.png)

Permettre l’accès CORS : votre configuration CORS devrait ressembler à ceci :

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

 Enfin, naviguez sur la page de paramètres W&B, `http(s)://YOUR-W&B-SERVER-HOST/admin-settings`. Activez l’option "Utiliser un backend de stockage de fichiers externe" \(Use an external file storage backend\), et indiquez le bucket s3, la région, et la file d’attente SQS dans le format suivant :

* **Bucket de Stockage de Fichiers**: `s3://<bucket-name>`
* **Région de Stockage de Fichiers**: `<region>`
*  **Abonnement Notifications**: `sqs://<queue-name>`

![Param&#xE8;tres de stockage de fichiers AWS](../.gitbook/assets/aws-filestore.png)

 Cliquez sur "Mettre les paramètres à jour et redémarrer W&B" \(Update settings and restart W&B\) pour appliquer les nouveaux paramètres.

###  Google Cloud Platform

Pour utiliser un bucket GCP Storage comme backend de stockage de fichiers pour W&B, vous aurez besoin de créer un bucket, ainsi qu’un sujet et un abonnement pubsub configurés pour recevoir les messages de création d’objet depuis ce bucket. 

**Créer un Sujet et un Abonnement Pubsub**

Naviguez jusqu’à Pub/Sub&gt;Sujets dans la Console GCP, et cliquez sur "Créer un sujet ". Choisissez un nom et créez un sujet.Puis, cliquez sur "Créer un abonnement " dans le tableau d’abonnements, en bas de la page. Choisissez un nom, et assurez-vous que le Type de Distribution soit paramétré sur "Pull". Cliquez sur "Créer".Assurez-vous que le compte de service ou que le compte sous lequel votre instance s’exécute a accès à cet abonnement. 

**Créer un Bucket de stockage**

Naviguez jusqu’à Stockage&gt;Navigateur dans la Console GCP, et cliquez sur "Créer un bucket" \(Create bucket\). Assurez-vous de choisir "Standard" pour la classe de stockage \(storage class\).

Assurez-vous que le compte de service ou que le compte sous lequel votre instance s’exécute a accès à cet abonnement. 

**Créer une Notification Pubsub**

Créer un fil de notification depuis le Bucket de Stockage pour le Sujet Pubsub ne peut malheureusement qu’être effectué dans la console. Assurez-vous d’avoir installé `gsutil et d’être connecté au bon Projet GCP, puis exécutez ce qui suit :`

```bash
gcloud pubsub topics list  # list names of topics for reference
gsutil ls                  # list names of buckets for reference

# create bucket notification
gsutil notification create -t <TOPIC-NAME> -f json gs://<BUCKET-NAME>
```

[Des références plus avancées sont disponibles sur le site web du Stockage Cloud.](https://cloud.google.com/storage/docs/reporting-changes)

**Ajouter des Autorisations Signées**

Pour créer des URL de fichiers signées, votre instance W&B a aussi besoin de l’autorisation iam.serviceAccounts.signBlob dans GCP. Vous pouvez l’ajouter en ajoutant le rôle Service Account Token Creator au compte de service ou au membre IAM sous lequel votre instance s’exécute. 

**Accorder des Autorisations au Node qui exécute W&B**

Le node sur lequel W&B Local s’exécute doit être configuré pour permettre l’accès au s3 et au sqs. En fonction du type de déploiement serveur pour lequel vous avez opté, vous pourrez avoir besoin d’ajouter les déclarations de police \(policy statements\) suivantes à votre rôle de node :

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

 **Configurer le Serveur W&B**

 Enfin, naviguez sur la page de paramètres W&B, `http(s)://YOUR-W&B-SERVER-HOST/admin-settings`. Activez l’option "Utiliser un backend de stockage de fichiers externe" \(Use an external file storage backend\), et indiquez le bucket s3, la région, et la file d’attente SQS dans le format suivant :

*  **Bucket de Stockage de fichiers**: `gs://<bucket-name>`
*  **Région de Stockage de fichiers** : blank
* **Abonnement Notifications**: `pubsub:/<project-name>/<topic-name>/<subscription-name>`

![Param&#xE8;tres de stockage de fichiers GCP](../.gitbook/assets/gcloud-filestore.png)

 Cliquez sur "Mettre les paramètres à jour et redémarrer W&B" \(Update settings and restart W&B\) pour appliquer les nouveaux paramètres.

### Azure

Pour créer un conteneur blob Azure comme stockage de fichiers pour W&B, il vous faudra créer un compte de stockage \(si vous n’en avez pas déjà un que vous souhaitez utiliser\), créer un conteneur blob et une file d’attente \(queue\) à l’intérieur de ce compte de stockage, puis créer un événement d’inscription qui envoie des notifications "blob créé" \(blob created\) à la file d’attente depuis le conteneur blob.

#### **Créer un Compte de Stockage**

Si vous avez déjà un compte de stockage que vous voulez utiliser, vous pouvez passer cette étape.

Rendez-vous sur [Comptes de stockage &gt; Ajouter](https://portal.azure.com/#create/Microsoft.StorageAccount) \(Storage Accounts &gt; Add\) dans le portail Azure. Sélectionnez un abonnement Azure, et sélectionnez n’importe quel groupe de ressources ou créez-en un nouveau. Entrez un nom pour votre compte de stockage.

![Mise en place du compte de stockage Azure](../.gitbook/assets/image%20%28106%29.png)

 Cliquez sur Vérifier + Créer \(Review + create\), puis, sur l’écran de résumé, cliquez sur Créer \(Create\) :

![V&#xE9;rification des d&#xE9;tails de compte de stockage Azure](../.gitbook/assets/image%20%28114%29.png)

#### **Créer le conteneur blob**

Rendez-vous sur [Comptes de Stockage](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Storage%2FStorageAccounts) sur le portail Azure, et cliquez sur votre nouveau compte de stockage. Dans le tableau de bord du compte de stockage, cliquez sur Service Blob &gt; Conteneurs \(Blob service &gt; Containers\) dans le menu :

![](../.gitbook/assets/image%20%28102%29.png)

 Créez un nouveau conteneur, et réglez-le sur Privé \(Private\) :

![](../.gitbook/assets/image%20%28110%29.png)

  
![](blob:null/ef0bf807-a139-458a-9200-1a2c391a860a)Rendez-vous dans Paramètres&gt;CORS&gt;Service Blob, et entrez l’IP de votre serveur wandb en tant qu’origine autorisée \(allowed origin\), avec les méthodes autorisées `GET` et `PUT`, et tous les en-têtes autorisés et exposés, puis enregistrez vos paramètres CORS.

![](../.gitbook/assets/image%20%28119%29.png)

#### **Créer la File d’attente**

Allez dans Service de File d’attente&gt;Files d’attente \(Queue Service &gt; Queues\) dans votre compte de stockage, et créez une nouvelle file d’attente :

![](../.gitbook/assets/image%20%28101%29.png)

 Allez dans Événements \(Events\) dans votre compte de stockage, et créez un abonnement à un événement :

![](../.gitbook/assets/image%20%28108%29.png)

Donnez à l’abonnement d’événement le Schéma d’événement "Event Grid", filtrez pour n’avoir que le type d’événement "Blob Créé" \(Blob Created\), paramétrez le Type de Point de terminaison \(Endpoint Type\) sur Files d’attente de Stockage \(Storage Queues\), puis sélectionnez le compte de stockage/la file d’attente comme point de terminaison.

![](../.gitbook/assets/image%20%28116%29.png)

Dans l’onglet Filtres, activez le filtrage de sujet pour les sujets qui commencent avec`/blobServices/default/containers/your-blob-container-name/blobs/`

![](../.gitbook/assets/image%20%28105%29.png)

#### **Configurer le Serveur W&B**

Allez dans Paramètres &gt; Afficher les clefs dans votre compte de stockage, cliquez sur "Affichez les clefs" \(Show keys\), puis copiez soit key1 &gt; Key soit key2 &gt; Key. Paramétrez cette clef sur votre serveur W&B comme la variable d’environnement `AZURE_STORAGE_KEY`.

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

Maintenant que l’application Slack est prête, il faut que nous autorisions l’utilisation d’un bot OAuth. Sélectionnez **OAuth & Permissions** dans la barre latérale, sur la gauche.

![](../.gitbook/assets/image%20%28125%29.png)

Sous **Scopes**, fournissez au bot le scope **incoming\_webhook.**

![](../.gitbook/assets/image%20%28128%29%20%281%29.png)

Enfin, configurez l’**URL de Redirection** \(Redirect URL\) pour pointer vers votre installation W&B. Vous devriez utiliser la même valeur que celle paramétrée pour votre **Hôte Frontend** \(Frontend Host\) dans vos paramètres de système locaux. Vous pouvez spécifier plusieurs URL si vous avez des mappings DNS différents dans votre instance.

![](../.gitbook/assets/image%20%28127%29.png)

Cliquez sur **Sauvegarder URLs** \(Save URLs\) une fois que vous avez terminé.Pour sécuriser davantage votre application Slack et évitez les abus, vous pouvez spécifier une plage d’IP sous 

**Restriction d’Usage de Token API** \(Restrick API Token Usage\) pour whitelister l’IP ou la plage d’IP de votre ou vos instances W&B. 

**Enregistrer votre application Slack avec W&B**Rendez-vous sur la page de **Paramètres Système** \(System Settings\) de votre instance W&B. Cochez la case pour permettre une application Slack personnalisée :

![](../.gitbook/assets/image%20%28126%29.png)

Il vous faudra fournir l’ID client de votre application Slack et son secret, que vous pouvez trouver dans l’onglet **Informations de Base** \(Basic Information\).

![](../.gitbook/assets/image%20%28120%29.png)

 C’est tout ! Vous pouvez maintenant vérifier que tout fonctionne en mettant en place une intégration Slack dans l’app W&B. Visitez [cette page](https://docs.wandb.ai/app/features/alerts) pour avoir des informations plus détaillées.

