# SimpleTransformers

Esta biblioteca está basada en la biblioteca Transformers de HuggingFace. Simple Transformers te permiten entrenar y evaluar rápidamente a los modelos de Transformer. Sólo son necesarias 3 líneas de código para inicializar un modelo, entrenarlo, y evaluarlo. Soporta Clasificación de Secuencias, Clasificación de Tokens \(NER\), Resolución de Preguntas, Mejoras del Modelo Idiomático, Entrenamiento del Modelo Idiomático, Generación Idiomática, Modelo T5, Tareas Seq2Seq, Clasificación Multimodal, e Inteligencia Artificial Conversacional.

##  El framework de Weights & Biases

Weights and Biases es soportado para visualizar el entrenamiento del modelo. Para usar esta característica, simplemente establece un nombre de proyecto para W&B en el atributo `wandb_project` del diccionario `args`. Esto registrará todos los valores de los hiperparámetros, las pérdidas del entrenamiento, y las métricas de evaluación para el proyecto dado.

```text
model = ClassificationModel('roberta', 'roberta-base', args={'wandb_project': 'project-name'})
```

Cualquier argumento adicional que vaya a `wandb.init` puede ser pasado como `wandb_kwargs`.

## Estructura

La biblioteca está diseñada para tener una clase separada por cada tarea NLP. Las clases que proveen funcionalidad similar se agrupan juntas.

* `simpletransformers.classification` - Incluye todos los modelos de Clasificación.
  * `ClassificationModel`
  * `MultiLabelClassificationModel`
* `simpletransformers.ner` - Incluye todos los modelos de Reconocimiento de Entidades Nombradas.
  * `NERModel`
* `simpletransformers.question_answering` - ncluye todos los modelos de Resolución de Preguntas.
  * `QuestionAnsweringModel`

Aquí hay algunos ejemplos mínimos.

##  Clasificación Multietiqueta

```text
  model = MultiLabelClassificationModel("distilbert","distilbert-base-uncased",num_labels=6,
    args={"reprocess_input_data": True, "overwrite_output_dir": True, "num_train_epochs":epochs,'learning_rate':learning_rate,
                'wandb_project': "simpletransformers"},
  )
   # Train the model
  model.train_model(train_df)

  # Evaluate the model
  result, model_outputs, wrong_predictions = model.eval_model(eval_df)
```

Aquí hay algunas visualizaciones generadas a partir del script de entrenamiento anterior, después de ejecutar un barrido de hiperparámetros.

[![](https://camo.githubusercontent.com/3beab1ca06813523711ff7750cb592430b786834/68747470733a2f2f692e696d6775722e636f6d2f6f63784e676c642e706e67)](https://camo.githubusercontent.com/3beab1ca06813523711ff7750cb592430b786834/68747470733a2f2f692e696d6775722e636f6d2f6f63784e676c642e706e67)

[![](https://camo.githubusercontent.com/b864ca220ddd4228027743790ac30741d1f435ad/68747470733a2f2f692e696d6775722e636f6d2f5252423432374d2e706e67)](https://camo.githubusercontent.com/b864ca220ddd4228027743790ac30741d1f435ad/68747470733a2f2f692e696d6775722e636f6d2f5252423432374d2e706e67)

##  Resolución de Preguntas

```text
  train_args = {
    'learning_rate': wandb.config.learning_rate,
    'num_train_epochs': 2,
    'max_seq_length': 128,
    'doc_stride': 64,
    'overwrite_output_dir': True,
    'reprocess_input_data': False,
    'train_batch_size': 2,
    'fp16': False,
    'wandb_project': "simpletransformers"
}

model = QuestionAnsweringModel('distilbert', 'distilbert-base-cased', args=train_args)
model.train_model(train_data)
```

Aquí hay algunas visualizaciones generadas a partir del script de entrenamiento anterior, después de ejecutar un barrido de hiperparámetros.

[![](https://camo.githubusercontent.com/1411cacec6226ebfa23c2e2dddc76ff5e41c136d/68747470733a2f2f692e696d6775722e636f6d2f7664636d7855532e706e67)](https://camo.githubusercontent.com/1411cacec6226ebfa23c2e2dddc76ff5e41c136d/68747470733a2f2f692e696d6775722e636f6d2f7664636d7855532e706e67)

[![](https://camo.githubusercontent.com/b8e12316520d4ad6d16449db2d13ab70e4d4a6e9/68747470733a2f2f692e696d6775722e636f6d2f395732775677732e706e67)](https://camo.githubusercontent.com/b8e12316520d4ad6d16449db2d13ab70e4d4a6e9/68747470733a2f2f692e696d6775722e636f6d2f395732775677732e706e67)

SimpleTransformers provee clases, así también como scripts de entrenamiento, para todas las tareas comunes relacionadas al lenguaje natural. Aquí hay una lista completa de los argumentos globales que son soportados por la biblioteca, con sus argumentos por defecto.

```text
global_args = {
  "adam_epsilon": 1e-8,
  "best_model_dir": "outputs/best_model",
  "cache_dir": "cache_dir/",
  "config": {},
  "do_lower_case": False,
  "early_stopping_consider_epochs": False,
  "early_stopping_delta": 0,
  "early_stopping_metric": "eval_loss",
  "early_stopping_metric_minimize": True,
  "early_stopping_patience": 3,
  "encoding": None,
  "eval_batch_size": 8,
  "evaluate_during_training": False,
  "evaluate_during_training_silent": True,
  "evaluate_during_training_steps": 2000,
  "evaluate_during_training_verbose": False,
  "fp16": True,
  "fp16_opt_level": "O1",
  "gradient_accumulation_steps": 1,
  "learning_rate": 4e-5,
  "local_rank": -1,
  "logging_steps": 50,
  "manual_seed": None,
  "max_grad_norm": 1.0,
  "max_seq_length": 128,
  "multiprocessing_chunksize": 500,
  "n_gpu": 1,
  "no_cache": False,
  "no_save": False,
  "num_train_epochs": 1,
  "output_dir": "outputs/",
  "overwrite_output_dir": False,
  "process_count": cpu_count() - 2 if cpu_count() > 2 else 1,
  "reprocess_input_data": True,
  "save_best_model": True,
  "save_eval_checkpoints": True,
  "save_model_every_epoch": True,
  "save_steps": 2000,
  "save_optimizer_and_scheduler": True,
  "silent": False,
  "tensorboard_dir": None,
  "train_batch_size": 8,
  "use_cached_eval_features": False,
  "use_early_stopping": False,
  "use_multiprocessing": True,
  "wandb_kwargs": {},
  "wandb_project": None,
  "warmup_ratio": 0.06,
  "warmup_steps": 0,
  "weight_decay": 0,
}
```

Consulta a [simpletransformers en github](https://github.com/ThilinaRajapakse/simpletransformers) para obtener una documentación más detallada.

Comprueba [este reporte de Weights and Biases](https://app.wandb.ai/cayush/simpletransformers/reports/Using-simpleTransformer-on-common-NLP-applications---Vmlldzo4Njk2NA) que cubre los transformadores de entrenamiento sobre algunos de los conjuntos de datos de referencia de GLUE más populares. [![Open In Colab](https://camo.githubusercontent.com/52feade06f2fecbf006889a904d221e6a730c194/68747470733a2f2f636f6c61622e72657365617263682e676f6f676c652e636f6d2f6173736574732f636f6c61622d62616467652e737667)](https://colab.research.google.com/drive/1oXROllqMqVvBFcPgTKJRboTq96uWuqSz?usp=sharing)

