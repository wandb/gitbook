# SimpleTransformers

Cette bibliothèque est basée sur la bibliothèque Transformers de HuggingFace. Simple Transformers vous permet d’entraîner et d’évaluer rapidement vos modèles Transformer. Il ne faut que 3 lignes de code pour initialiser un modèle, l’entraîner et l’évaluer. Sont pris en charge : la classification de séquences, la classification de tokens \(NER\), les systèmes de questions-réponses, l’ajustement du langage du modèle, l’entraînement de langage du modèle, la génération de langage, le modèle T5, les tâches Seq2Seq, la classification multimodale et l’intelligence artificielle conversationnelle.

## **Le framework Weights & Biases**

Weights and Biases est pris en charge pour visualiser l’entraînement de modèle. Pour utiliser cette fonction, paramétrez simplement un nom de projet pour W&B dans l’attribut wandb\_project du dictionnaire des args. Cela enregistrera toutes les valeurs d’hyperparamètres, de pertes d’entraînement et de mesures d’évaluation sous le projet donné.

```text
model = ClassificationModel('roberta', 'roberta-base', args={'wandb_project': 'project-name'})
```

 Tout argument supplémentaire qui est entré dans `wandb.init` peut être ajouté comme `wandb_kwargs`.

## Structure

 Cette bibliothèque est conçue pour avoir une classe séparée pour chaque tâche de traitement naturel de langage \(NLP\). Les classes qui fournissent des fonctionnalités similaires sont regroupées ensemble.

* `simpletransformers.classification` - Includes all Classification models.
  * `ClassificationModel`
  * `MultiLabelClassificationModel`
* `simpletransformers.ner` - Includes all Named Entity Recognition models.
  * `NERModel`
* `simpletransformers.question_answering` - Includes all Question Answering models.
  * `QuestionAnsweringModel`

Voici quelques exemples minimes.

##  **Classification multi-étiquettes**

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

Voici quelques visualisations générées par le script d’entraînement ci-dessus après avoir exécuté un balayage d’hyperparamètres.

[![](https://camo.githubusercontent.com/3beab1ca06813523711ff7750cb592430b786834/68747470733a2f2f692e696d6775722e636f6d2f6f63784e676c642e706e67)](https://camo.githubusercontent.com/3beab1ca06813523711ff7750cb592430b786834/68747470733a2f2f692e696d6775722e636f6d2f6f63784e676c642e706e67)

[![](https://camo.githubusercontent.com/b864ca220ddd4228027743790ac30741d1f435ad/68747470733a2f2f692e696d6775722e636f6d2f5252423432374d2e706e67)](https://camo.githubusercontent.com/b864ca220ddd4228027743790ac30741d1f435ad/68747470733a2f2f692e696d6775722e636f6d2f5252423432374d2e706e67)

## **Modèles de questions-réponses**

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

Voici quelques visualisations générées par le script d’entraînement ci-dessus après avoir exécuté un balayage d’hyperparamètres.

[![](https://camo.githubusercontent.com/1411cacec6226ebfa23c2e2dddc76ff5e41c136d/68747470733a2f2f692e696d6775722e636f6d2f7664636d7855532e706e67)](https://camo.githubusercontent.com/1411cacec6226ebfa23c2e2dddc76ff5e41c136d/68747470733a2f2f692e696d6775722e636f6d2f7664636d7855532e706e67)

[![](https://camo.githubusercontent.com/b8e12316520d4ad6d16449db2d13ab70e4d4a6e9/68747470733a2f2f692e696d6775722e636f6d2f395732775677732e706e67)](https://camo.githubusercontent.com/b8e12316520d4ad6d16449db2d13ab70e4d4a6e9/68747470733a2f2f692e696d6775722e636f6d2f395732775677732e706e67)

  
 Simple Transformers fournit des classes ainsi que des scripts d’entraînement pour toutes les tâches communes du langage naturel. Voici une liste complète des arguments globaux qui sont pris en charge par cette bibliothèque, avec les arguments par défaut.

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

 Référez-vous à [simpletransformers sur github](https://github.com/ThilinaRajapakse/simpletransformers) pour avoir une documentation plus détaillée.

Jetez un œil sur ce [rapport Weights and Biases](https://app.wandb.ai/cayush/simpletransformers/reports/Using-simpleTransformer-on-common-NLP-applications---Vmlldzo4Njk2NA) qui traite des transformers d’entraînement sur les datasets de benchmark GLUE les plus utilisés. Essayez-le par vous-même dans un colab. [Ouvrir le colab](https://colab.research.google.com/drive/1oXROllqMqVvBFcPgTKJRboTq96uWuqSz?usp=sharing) [![Open In Colab](https://camo.githubusercontent.com/52feade06f2fecbf006889a904d221e6a730c194/68747470733a2f2f636f6c61622e72657365617263682e676f6f676c652e636f6d2f6173736574732f636f6c61622d62616467652e737667)](https://colab.research.google.com/drive/1oXROllqMqVvBFcPgTKJRboTq96uWuqSz?usp=sharing)

