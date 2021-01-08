---
description: "命令行接口登录, 还原代码状态, 将本地目录同步到我们的服务器, 并使用我们的命令行接口运行超参数扫描（hyperparameter sweeps运行 pip install wandb 命令后，你将拥有一个新命令, wandb 可以使用。可以使用以下子命令:子命令\t说明docs\t在浏览器中打开文档init\t用 W&B 配置一个目录login\t登录 W&Boffline\t只将运行"
---

# Command Line Interface

运行 `pip install wandb` 命令后，你将拥有一个新命令, **wandb 可以使用。**

可以使用以下子命令:

| 子命令 | 说明 |
| :--- | :--- |
| docs | 在浏览器中打开文档 |
| init | 用 W&B 配置一个目录 |
| login | 登录 W&B |
| offline | 只将运行数据保存在本地, 不进行云端同步\(`off` 已废弃\) |
| online | 确保在词目录下启用W&B \(`on` 已废弃\) |
| disabled | 禁用所有API 调用, 对测试有用 |
| enabled | 和 `online`一样，一旦你完成测试后，恢复正常的W&B记录 |
| docker | 运行docker镜像, 挂载cwd, 并确保安装了wandb |
| docker-run | 将W&B环境变量添加到docker run 命令 |
| projects | 列出项目 |
| pull | 从W&B拉取运行的文件 |
| restore | 还原运行的代码和配置状态 |
| run | 启动一个非Python程序, 对于Python来说使用wandb.init\(\) |
| runs | 列出一个项目中的运行 |
| sync | 同步包含tfevents 或以前运行文件的本地目录 |
| status | 列出当前目录状态 |
| sweep | 给一个YAML定义，创建一个新的扫描 |
| agent | 启动一个代理以在扫描中运行程序 |

## **还原你的代码状态**

使用（还原）`restore` 来还原你的代码运行时的状态

###  **示例**

```python
# creates a branch and restores the code to the state it was in when run $RUN_ID was executed
wandb restore $RUN_ID
```

 **我们如何捕获代码的状态?**

 当你脚本中的`wandb.init` 被调用时，如果代码是在一个git仓库中，则会保存一个链接到最新的git commit。如果有未提交的改动或与远程不同步的改动，也会创建一个差异补丁（diff patch）。

