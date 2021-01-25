# Sécurité

 Pour des raisons de simplicité, W&B utilise des clefs API pour autorisation lorsqu’il y a un accès à l’API. Vous pouvez trouver vos clefs API dans vos [paramètres](https://app.wandb.ai/settings). Votre clef API devrait être stockée de manière sécurisée, et ne jamais être entrée dans le contrôle de version. En plus de vos clefs API personnelles, vous pouvez ajouter des utilisateurs de Compte de Service à votre équipe.

## Rotation de clef

Vos clefs de compte personnel et de service peuvent effectuer une rotation ou être révoquées. Créez simplement une nouvelle Clef API ou un nouvel utilisateur de Compte de Service et reconfigurez vos scripts pour utiliser cette nouvelle clef. Une fois que tous vos processus sont reconfigurés, vous pouvez enlever votre ancienne clef API de votre profil ou de votre équipe.

##  Passer d’un compte à l’autre

Si vous avez deux comptes W&B qui travaillent depuis la même machine, vous aurez besoin d’une manière pratique de passer d’une clef API à l’autre. Vous pouvez stocker les deux clefs API dans un fichier de votre machine, puis ajouter un code comme celui qui suit à vos répertoires. Cela permet d’éviter d’inscrire votre clef secrète dans un système de contrôle source, ce qui est potentiellement dangereux.

```text
if os.path.exists("~/keys.json"):
   os.environ["WANDB_API_KEY"] = json.loads("~/keys.json")["work_account"]
```

