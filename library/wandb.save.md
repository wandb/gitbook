---
description: 把当前运行项的关联文件保存至云端。
---

# wandb.save\(\)

有两种方式保存当前运行项的关联文件。

1.  用`wandb.save(filename)`；
2. 把文件放在wandb运行项路径，运行项结束时就会上传该文件。

{% hint style="info" %}
 如果你要做[断点续训](https://app.gitbook.com/@weights-and-biases/s/docs/library/resuming)，调用wandb.restore\(filename\)即可恢复关联文件
{% endhint %}

你如果想在写入文件的同时就同步文件，可以在`wandb.save`指定一个文件名或通配符。

## **wandb.save示例**

 若要查看完整示例，请看[这篇报告](https://wandb.ai/lavanyashukla/save_and_restore/reports/Saving-and-Restoring-Models-with-W&B--Vmlldzo3MDQ3Mw)。

```python
# Save a model file from the current directory
wandb.save('model.h5')

# Save all files that currently exist containing the substring "ckpt"
wandb.save('../logs/*ckpt*')

# Save any files starting with "checkpoint" as they're written to
wandb.save(os.path.join(wandb.run.dir, "checkpoint*"))
```

{% hint style="info" %}
默认情况下，权阈的本地运行路径位于脚本的相对路径./wandb，路径名格式如run-20171023\_105053-3o4933r0，其中20171023\_105053是时间戳，3o4933r0是运行项id。你可以设置环境变量WANDB\_DIR或wandb.init的关键字参数dir，设置为绝对路径，文件就会写入该路径。
{% endhint %}

##  **把文件保存至wandb运行项路径示例**

 文件“model.h5”被保存在wandb.run.dir，训练结束后将被上传。

```python
import wandb
wandb.init()

model.fit(X_train, y_train,  validation_data=(X_test, y_test),
    callbacks=[wandb.keras.WandbCallback()])
model.save(os.path.join(wandb.run.dir, "model.h5"))
```

 有一个公共示例页面。你可以看到，在“文件”选项卡有个model-best.h5。那是Keras集成默认自动保存的，但你可以手动保存检查点，我们会为你保存起来，并且和你的运行项关联起来。

 [查看动态示例→](https://wandb.ai/wandb/neurips-demo/runs/206aacqo/files)

![](../.gitbook/assets/image%20%2844%29.png)

##  **常见问题**

###  **忽略了某些文件**

 ****你可以编辑文件`wandb/settings`并设置ignore\_globs等于某个[通配符](https://en.wikipedia.org/wiki/Glob_%28programming%29)列表，通配符列表要用逗号分开。还可以设置环境变量**WANDB\_IGNORE\_GLOBS**。一个常见的使用情景就是防止git补丁被上传，因为git补丁是我们自动创建的，即**WANDB\_IGNORE\_GLOBS=\*.patch**

###  **运行结束前就同步文件**

如果你的运行时间较长，你可能想在运行结束前就把文件上传到云端，这些文件例如模型检查点。而默认情况下，直到运行结束我们才上传大部分文件。那么，你可以在脚本中插入`wandb.save('*.pth')`或`wandb.save('latest.pth')`，每当这些文件被写入或更新就会上传。

###  **更改文件保存路径**

如果你默认把文件保存至AWS S3或谷歌云存储，你可能会收到这样的错误：`events.out.tfevents.1581193870.gpt-tpu-finetune-8jzqk-2033426287是一个云存储网址，无法将文件保存至wandb`

### **获取运行项名称**

如果你要在脚本中使用运行项名称，就可以用`wandb.run.name`，会得到运行项名称——例如“blissful-waterfall-2”。你要先为run对象调用save，然后才能获得显示的名称：

you need to call save on the run before being able to access the display name:

```text
run = wandb.init(...)
run.save()
print(run.name)
```

### **把保存的全部文件推送至wandb**

 在脚本开头wandb.init后面调用一次`wandb.save("*.pt")`，那么，与该模式匹配的所有文件，被写入wandb.run.dir后就会立即保存。

###  **本地文件同步到云存储后，删除本地文件**

   本地文件同步到云存储后，如果要删除本地文件，可以运行命令`wandb gc`。详细使用方法请查看\`wandb gc —help 

