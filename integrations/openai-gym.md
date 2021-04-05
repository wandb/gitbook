# OpenAI Gym

Si estás utilizando [OpenAI Gym](https://gym.openai.com/) registraremos automáticamente videos de tu entorno que sean generados por gym.wrappers.Monitor. Solamente establece el argumento de palabra clave monitor\_gym para `wandb.init` a True, o llama a `wandb.gym.monitor()`.

Aquí hay un pequeño proyecto de ejemplo que utiliza OpenAI Gym para jugar a Snake: [https://github.com/Xyzrr/rl-snake](https://github.com/Xyzrr/rl-snake)

