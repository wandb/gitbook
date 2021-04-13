# Resuming

你可以通过向 `wandb.init()` 传递`resume=True`来让wandb自动恢复运行。如果你的进程没有成功退出，你下次运行它时，wandb将从最后一步开始记录。下面是 Keras中的一个简单例子:

```python
import keras
import numpy as np
import wandb
from wandb.keras import WandbCallback
wandb.init(project="preemptable", resume=True)

if wandb.run.resumed:
    # restore the best model
    model = keras.models.load_model(wandb.restore("model-best.h5").name)
else:
    a = keras.layers.Input(shape=(32,))
    b = keras.layers.Dense(10)(a)
    model = keras.models.Model(input=a,output=b)

model.compile("adam", loss="mse")
model.fit(np.random.rand(100, 32), np.random.rand(100, 10),
    # set the resumed epoch
    initial_epoch=wandb.run.step, epochs=300,
    # save the best model if it improved each epoch
    callbacks=[WandbCallback(save_model=True, monitor="loss")])
```

 只有当进程在与失败进程相同的文件系统上重新启动时，自动断点续训才会生效。如果你不能共享一个文件系统，我们允许你设置**WANDB\_RUN\_ID**: 一个全局唯一的字符串 \(每个项目范围内\) 对应于你的脚本的一个运行。它不得长于64个字符。所有非单词字符将被转换为连字符。

```python
# store this id to use it later when resuming
id = wandb.util.generate_id()
wandb.init(id=id, resume="allow")
# or via environment variables
os.environ["WANDB_RESUME"] = "allow"
os.environ["WANDB_RUN_ID"] = wandb.util.generate_id()
wandb.init()
```

如果你把**WANDB\_RESUME** 设置为 "allow",你可以随时把**WANDB\_RUN\_ID** 设置为一个唯一的字符串，进程的重新启动将被自动处理。如果你将 **WANDB\_RESUME** 设置为"must", 如果要恢复的运行不存在，wandb 将抛出一个错误，而不是自动创建一个新的运行。

| 方法 | 语法 | 永不断点续训\(默认\) | 总是断点续训 | 断点续训指定运行ID | 从同一目录断点续训 |
| :--- | :--- | :--- | :--- | :--- | :--- |
|  环境 | WANDB\_RESUME= | "never" | "must" | "allow" \(Requires WANDB\_RUN\_ID=RUN\_ID\) | \(无效\) |
| init | wandb.init\(resume=\) | ​ | \(not available\) | resume=RUN\_ID | \(无效\) |

如果多个进程同时使用相同的run\_id,记录的结果会出乎意料并发生速率限制

如果你断点续训一个运行，并且在`wandb.init() 中指定了注解`, 这些注解将会覆盖你在UI中添加的任何注解。

注意:作为[扫描](https://docs.wandb.ai/v/zh-hans/sweeps-1)部分执行的运行不支持断点续训。

