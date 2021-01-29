# config

<!-- Insert buttons and diff -->


[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_config.py#L24-L263)




Config object

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>wandb.config() -> None
</code></pre>



<!-- Placeholder for "Used in" -->

Config objects are intended to hold all of the hyperparameters associated with
a wandb run and are saved with the run object when <a href="../wandb/init.md"><code>wandb.init</code></a> is called.

We recommend setting <a href="../wandb/config.md"><code>wandb.config</code></a> once at the top of your training experiment or
setting the config as a parameter to init, ie. <a href="../wandb/init.md"><code>wandb.init(config=my_config_dict)</code></a>

You can create a file called `config-defaults.yaml`, and it will automatically be
loaded into <a href="../wandb/config.md"><code>wandb.config</code></a>. See https://docs.wandb.com/library/config#file-based-configs.

You can also load a config YAML file with your custom name and pass the filename
into `wandb.init(config="special_config.yaml")`.
See https://docs.wandb.com/library/config#file-based-configs.

#### Examples:

Basic usage
```
wandb.config.epochs = 4
wandb.init()
for x in range(wandb.config.epochs):
    # train
```

Using wandb.init to set config
```
wandb.init(config={"epochs": 4, "batch_size": 32})
for x in range(wandb.config.epochs):
    # train
```

Nested configs
```
wandb.config['train']['epochs] = 4
wandb.init()
for x in range(wandb.config['train']['epochs']):
    # train
```

Using absl flags
```
flags.DEFINE_string(‘model’, None, ‘model to run’) # name, default, help
wandb.config.update(flags.FLAGS) # adds all absl flags to config
```

Argparse flags
```
wandb.init()
wandb.config.epochs = 4

parser = argparse.ArgumentParser()
parser.add_argument('-b', '--batch-size', type=int, default=8, metavar='N',
                    help='input batch size for training (default: 8)')
args = parser.parse_args()
wandb.config.update(args)
```

Using TensorFlow flags (deprecated in tensorflow v2)
```
flags = tf.app.flags
flags.DEFINE_string('data_dir', '/tmp/data')
flags.DEFINE_integer('batch_size', 128, 'Batch size.')
wandb.config.update(flags.FLAGS)  # adds all of the tensorflow flags to config
```
