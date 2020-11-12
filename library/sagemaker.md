# Sagemaker

##  **集成SageMaker**

 权阈集成[亚马逊SageMaker](https://aws.amazon.com/sagemaker/)，能够自动地读取超参数、对分布式运行项分组、从检查点开始断点续训。

###  **验证**

 权阈寻找一个文件`secrets.env`，该文件位于训练脚本的相对路径，当调用wandb.init\(\)时，权阈把它们载入环境中。在你启动实验的脚本中，调用`wandb.sagemaker_auth(path="source_dir")`，即可生成一个文件`secrets.env`。一定要把该文件添加到.`gitignore`！

### **现有估计器**

 如果你用的是SageMaker预设估计器，就需要向源文件路径添加一个`requirements.txt`，wandb就位于源文件路径。

```text
wandb
```

如果你用的是运行Python 2的估计器，就需要从轮子（[wheel](https://pythonwheels.com/)）直接安装psutil，然后才能安装wandb：

```text
https://wheels.galaxyproject.org/packages/psutil-5.4.8-cp27-cp27mu-manylinux1_x86_64.whl
wandb
```

上有一个完整范例，还可到我们的[论坛](https://www.wandb.com/blog/running-sweeps-with-sagemaker)详细了解。

