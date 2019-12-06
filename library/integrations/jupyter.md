# Jupyter

### Jupyter Integration

Our tools work well with Jupyter notebooks. Each time you call **wandb.init** we create a new run and print a link to the run page. If the machine or directory you are running `jupyter notebook` from isn't configured, you will be prompted to configure the directory interactively in the notebook.

You can call **wandb.log** as you would normally and metrics will be sent to the run created by **wandb.init**. If you want to display live results in the notebook, you can decorate the cell that calls `wandb.log` with **%%wandb**. If you run this cell multiple times, data will be appended to the run.

{% hint style="info" %}
Call **wandb.init** before you use **%%wandb**. If you are calling **wandb.init** inside a model training function, add a line after **wandb.init** to show graphs in the : **display\(wandb.jupyter.Run\(\)\)**
{% endhint %}

### Google Colab

We have a nice extra integration with Google Colab. The first time you call `wandb.init` we will automatically pull in your credentials if you're already logged into wandb.

### Launching Jupyter

Calling `wandb docker --jupyter` will launch a docker container, mount your code in it, ensure jupyter is installed and launch it on port 8888.

### Sharing Notebooks

If your project is private, viewers of your notebook will be prompted to login to view results.

