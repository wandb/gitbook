# keras

<!-- Insert buttons and diff -->


[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/latest/wandb/integration/keras/__init__.py)



Tools for integrating `wandb` with [`Keras`](https://keras.io/),

a deep learning API for [`TensorFlow`](https://www.tensorflow.org/).

## Classes

[`class WandbCallback`](./wandbcallback.md): `WandbCallback` automatically integrates keras with wandb.

[`class WandbEvalCallback`](./wandbevalcallback.md): Abstract base class to build Keras callbacks for model prediction visualization.

[`class WandbMetricsLogger`](./wandbmetricslogger.md): `WandbMetricsLogger` automatically logs the `logs` dictionary

[`class WandbModelCheckpoint`](./wandbmodelcheckpoint.md): `WandbModelCheckpoint` periodically saves a Keras model or model weights

