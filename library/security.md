# Sécurité

 Pour plus de simplicité, W&B utilise des clefs API pour l’autorisation d’un accès à l’API. Vous pouvez trouver vos clefs API dans vos [paramètres](https://app.wandb.ai/settings). Votre clef API doit être stockée de manière sécurisée, et ne jamais être entrée dans le système de gestion de version. En plus de vos clefs API personnelles, vous pouvez ajouter des utilisateurs de compte de service à votre équipe.

## Rotation de clef

Vos clefs de compte personnel et de service peuvent effectuer une rotation ou être révoquées. Pour cela, il suffit de créer une nouvelle clef API ou un nouvel utilisateur de compte de service et reconfigurer vos scripts pour utiliser cette nouvelle clef. Une fois que tous vos processus sont reconfigurés, vous pouvez supprimer votre ancienne clef API de votre profil ou de votre équipe.

##  Passer d’un compte à l’autre

Si vous avez deux comptes W&B qui utilisent sur le même ordinateur, vous aurez besoin d’une manière pratique de passer d’une clef API à l’autre. Vous pouvez stocker les deux clefs API dans un fichier de votre ordinateur, puis ajouter un code similaire au code ci-dessous à vos répertoires. Cela permet d’éviter d’inscrire votre clef secrète dans un système de gestion de code source, ce qui est potentiellement dangereux.

```text
if os.path.exists("~/keys.json"):
   os.environ["WANDB_API_KEY"] = json.loads("~/keys.json")["work_account"]
```

