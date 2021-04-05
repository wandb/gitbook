# Security

为了简单起见,在访问API时W&B使用了API密匙进行授权。你可以在你的[设置](https://app.wandb.ai/settings)中找到你的API密匙。你的API密匙要注意安全保存，永远不要检入到版本控制。除了个人API密匙外，你还可以将服务账户用户添加到你的团队中。

##  **密匙轮转** <a id="key-rotation"></a>

个人和服务账户的密匙都可以被轮转或撤销。只需创建一个新的API密匙或服务账户用户，然后重新配置你的脚本以使用新密匙。一旦所有流程都被重新配置，你就可以从你的主页或团队中删除旧API密匙。

## **在账户之间切换** <a id="switching-between-accounts"></a>

如果你有两个W&B账户在同一个机器上工作，你需要一个好的方法来切换不同的API密匙。你可以将两个API密匙都存储在你机器上的一个文件中，然后在你的仓库添加类似下面的代码。这是为了避免将你的密匙检如到源代码版本控制系统，那样会有潜在的安全问题。

```text
if os.path.exists("~/keys.json"):   os.environ["WANDB_API_KEY"] = json.loads("~/keys.json")["work_account"]
```

[  
](https://docs.wandb.ai/library/grouping)



