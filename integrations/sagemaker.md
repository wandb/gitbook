# SageMaker

## **SageMaker集成**

W&B与[亚马逊SageMaker](https://aws.amazon.com/sagemaker/)集成，自动地读取超参数、对分布式运行分组、从检查点开始断点续训。

### **认证**

当wandb.init\(\)被调用时，W&B会在训练脚本的相对路径下寻找一个`secrets.env`文件，并将其加载到环境中。你可以在你启动实验的脚本中，调用`wandb.sagemaker_auth(path="source_dir")`，来生成`secrets.env`文件。一定要把该文件添加到你的.`gitignore`！

**现有Estimators**

如果你用的是SageMaker预配置的Estimators，你需要向源代码文件路径添加一个`requirements.txt`，其中包含wandb。

```text
wandb
```

如果你用的是运行Python 2的Estimators，你需要在安装wandb之前直接从一个[轮子（wheel）](https://pythonwheels.com/)来安装 psutil

```text
https://wheels.galaxyproject.org/packages/psutil-5.4.8-cp27-cp27mu-manylinux1_x86_64.whl
wandb
```

[Github](https://github.com/wandb/examples/tree/master/examples/pytorch/pytorch-cifar10-sagemaker)上有一个完整示例，还可到我们的[论坛](https://www.wandb.com/blog/running-sweeps-with-sagemaker)详细了解。

