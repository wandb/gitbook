# Resuming

You can have wandb automatically resume runs by passing `resume=True` to `wandb.init()`. If your process doesn't exit successfully, the next time you run it wandb will start logging from the last step. Below is a simple example in Keras:

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

Automatic resuming only works if the process is restarted on top of the same filesystem as the failed process. If you can't share a filesystem, we allow you to set the **WANDB\_RUN\_ID**: a globally unique string \(per project\) corresponding to a single run of your script. It must be no longer than 64 characters. All non-word characters will be converted to dashes.

```python
# store this id to use it later when resuming
id = wandb.util.generate_id()
wandb.init(id=id, resume="allow")
# or via environment variables
os.environ["WANDB_RESUME"] = "allow"
os.environ["WANDB_RUN_ID"] = wandb.util.generate_id()
wandb.init()
```

If you set **WANDB\_RESUME** equal to "allow", you can always set **WANDB\_RUN\_ID** to a unique string and restarts of the process will be handled automatically. If you set **WANDB\_RESUME** equal to "must", wandb will throw an error if the run to be resumed does not exist yet instead of auto-creating a new run.

| Method | Syntax | Never Resume \(default\) | Always Resume | Resume specifying run id | Resume from same directory |
| :--- | :--- | :--- | :--- | :--- | :--- |
| environment | WANDB\_RESUME= | "never" | "must" | "allow" \(Requires WANDB\_RUN\_ID=RUN\_ID\) | \(not available\) |
| init | wandb.init\(resume=\) |  | \(not available\) | resume=RUN\_ID | resume=True |

{% hint style="warning" %}
If multiple processes use the same run\_id concurrently unexpected results will be recorded and rate limiting will occur.
{% endhint %}

{% hint style="info" %}
If you resume a run and you have **notes** specified in `wandb.init()`, those notes will overwrite any notes that you have added in the UI.
{% endhint %}

{% hint style="info" %}
Note that resuming a run which was executed as part of a [Sweep](../sweeps/) is not supported.
{% endhint %}

