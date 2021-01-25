---
description: >-
  Alertes programmables déclenchées depuis Python et envoyées par Slack ou par
  email
---

# wandb.alert\(\)

Envoyez une alerte Slack ou email déclenchée par votre script Python.

1.  [Activez les alertes sur votre compte →](https://docs.wandb.ai/app/features/alerts)
2.  [Essayez le code →](http://tiny.cc/wb-alerts)
3. Vérifiez votre Slack ou votre adresse email pour voir les alertes programmables.

### Arguments

`wandb.alert(title="Low Acc", text="Accuracy is below the expected threshold")`

* **title \(chaîne\)** : Une courte description de l’alerte, par exemple "Précision Basse"
* **text \(chaîne\)** : Une description plus longue et plus détaillée de ce qui a déclenché l’alerte
* **level \(optionnel\)** : Niveau d’importance de l’alerte – doit être soit `INFO` \(information\), soit `WARN` \(Avertissement\) soit `ERROR` \(Erreur\). 
*  **wait\_duration \(optionnel\)** : Combien de secondes doivent s’écouler avant qu’une nouvelle alerte avec le même **title** ne soit envoyée, pour réduire le spam d’alertes.

###  Exemple

 Cette alerte simple envoie un avertissement lorsque la précision tombe sous un certain seuil. Pour éviter le spam, il envoie ces alertes avec au moins 5 minutes d’écart.

[Essayer le code →](http://tiny.cc/wb-alerts)

```python
from datetime import timedelta
import wandb
from wandb import AlertLevel

if acc < threshold:
    wandb.alert(
        title='Low accuracy', 
        text=f'Accuracy {acc} is below the acceptable theshold {threshold}',
        level=AlertLevel.WARN,
        wait_duration=timedelta(minutes=5)
    )
```

