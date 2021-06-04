# OpenAI Gym

Si vous utilisez [OpenAI Gym](https://gym.openai.com/), nous enregistrerons automatiquement les vidéos de votre environnement générées par gym.wrappers.Monitor. Il suffit de paramétrer l’argument mot-clef **monitor\_gym** dans wandb.init sur **True** ou d’appeler `wandb.gym.monitor()`.

Voici un petit exemple de projet qui utilise OpenAi Gym pour jouer au jeu Snake : [https://github.com/Xyzrr/rl-snake](https://github.com/Xyzrr/rl-snake)

