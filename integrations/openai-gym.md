# OpenAI Gym

 如果你正在使用[OpenAI Gym](https://gym.openai.com/) ，我们会自动记录你的环境通过`gym.wrappers.Monitor`生成的视频。只要将`wandb.init`的**monitor\_gym** 关键字参数设置为True或调用`wandb.gym.monitor()`即可。

下面是一个使用OpenAI Gym来玩Snake的小示例项目：[https://github.com/Xyzrr/rl-snake](https://github.com/Xyzrr/rl-snake)

