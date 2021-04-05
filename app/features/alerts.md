---
description: >-
  Recevoir une notification Slack lorsque votre essai plante, s’achève, ou
  appelle wandb.alert().
---

# Alerts

W&B peut poster des notifications par email ou par Slack lorsque vos essais plantent, s’achèvent, ou appellent [wandb.alert\(\)](https://docs.wandb.ai/library/wandb.alert).

###  Alertes utilisateur

 Mettez en place des notifications lorsqu’un essai s’achève, plante, ou appelle `wandb.alert()`. Elles s’appliquent à tous les projets lorsque vous lancez des essais, ce qui inclue les projets personnels et d’équipe.

Dans vos [Paramètres Utilisateur ](https://wandb.ai/settings):

*  Descendez jusqu’à la section **Alerts**
* Cliquez sur **Connect Slack** pour choisir un canal sur lequel poster les alertes. Nous recommandons d’utiliser le canal de **Slackbot** car il garde les alertes privées.
* **Email** enverra un message à l’adresse email que vous avez utilisée pour vous enregistrer sur W&B. Nous recommandons de mettre en place un filtre dans votre boîte email pour que ces alertes aillent dans un dossier et ne remplisse pas votre boîte de réception.

![](../../.gitbook/assets/demo-connect-slack.png)

###  Alertes d’équipe

 Les administrateurs d’équipe peuvent mettre en place des alertes pour l’équipe sur la page de paramètres de l’équipe : wandb.ai/teams/`your-team`. Ces alertes s’appliquent à tous les membres de votre équipe. Nous recommandons d’utiliser le canal de **Slackbot** car il garde les alertes privées.

### Changer de canaux Slack

Pour changer le canal dans lequel vous postez, cliquez sur **Disconnect Slack** puis reconnectez-vous, en choisissant un

