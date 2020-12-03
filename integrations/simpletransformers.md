# SimpleTransformers

 このライブラリは、HuggingFaceによるTransformersライブラリに基づいています。Simple Transformersを使用すると、Transformerモデルをすばやくトレーニングして評価できます。モデルの初期化、モデルのトレーニング、モデルの評価に必要なコードは3行だけです。シーケンス分類、トークン分類（NER）、質問応答、言語モデルの微調整、言語モデルトレーニング、言語生成、T5モデル、Seq2Seqタスク、マルチモーダル分類、会話型AIをサポートします。

##  **Weights＆Biasesフレームワーク**

モデルトレーニングを視覚化するために、重みとバイアスがサポートされています。これを使用するには、argsディクショナリの`wandb_project`属性でW＆Bのプロジェクト名を設定するだけです。これにより、すべてのハイパーパラメータ値、トレーニングロス、および評価指標が特定のプロジェクトに記録されます。

```text
model = ClassificationModel('roberta', 'roberta-base', args={'wandb_project': 'project-name'})
```

 `wandb.init`に入る追加の引数は、`wandb_kwargs`として渡すことができます。

##  **構造**

ライブラリは、NLPタスクごとに個別のクラスを持つように設計されています。同様の機能を提供するクラスはグループ化されています。

* `simpletransformers.classification` - すべての分類モデルが含まれます。
  * `ClassificationModel`
  * `MultiLabelClassificationModel`
* `simpletransformers.ner` - すべての名前付きエンティティ認識モデルが含まれます。
* `simpletransformers.question_answering` - すべての質問応答モデルが含まれます。
  * `QuestionAnsweringModel`

 ここにいくつかの最低限の例があります

## **マルチラベル分類**

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

ハイパーパラメータスイープを実行した後、上記のトレーニングスクリプトから生成されたいくつかの視覚化を次に示します。

[![](https://camo.githubusercontent.com/3beab1ca06813523711ff7750cb592430b786834/68747470733a2f2f692e696d6775722e636f6d2f6f63784e676c642e706e67)](https://camo.githubusercontent.com/3beab1ca06813523711ff7750cb592430b786834/68747470733a2f2f692e696d6775722e636f6d2f6f63784e676c642e706e67)

[![](https://camo.githubusercontent.com/b864ca220ddd4228027743790ac30741d1f435ad/68747470733a2f2f692e696d6775722e636f6d2f5252423432374d2e706e67)](https://camo.githubusercontent.com/b864ca220ddd4228027743790ac30741d1f435ad/68747470733a2f2f692e696d6775722e636f6d2f5252423432374d2e706e67)

##  **質疑応答**

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

ハイパーパラメータスイープを実行した後、上記のトレーニングスクリプトから生成されたいくつかの視覚化を次に示します。

[![](https://camo.githubusercontent.com/1411cacec6226ebfa23c2e2dddc76ff5e41c136d/68747470733a2f2f692e696d6775722e636f6d2f7664636d7855532e706e67)](https://camo.githubusercontent.com/1411cacec6226ebfa23c2e2dddc76ff5e41c136d/68747470733a2f2f692e696d6775722e636f6d2f7664636d7855532e706e67)

[![](https://camo.githubusercontent.com/b8e12316520d4ad6d16449db2d13ab70e4d4a6e9/68747470733a2f2f692e696d6775722e636f6d2f395732775677732e706e67)](https://camo.githubusercontent.com/b8e12316520d4ad6d16449db2d13ab70e4d4a6e9/68747470733a2f2f692e696d6775722e636f6d2f395732775677732e706e67)

SimpleTransformersは、すべての一般的な自然言語タスク用のクラスとトレーニングスクリプトを提供します。ライブラリでサポートされているグローバル引数の完全なリストと、デフォルトの引数を次に示します。

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

 詳細なドキュメントについては、[githubのsimpletransformers](https://github.com/ThilinaRajapakse/simpletransformers)を参照してください

最も人気のあるGLUEベンチマークデータセットのトランスフォーマーのトレーニングをカバーする[このWeights and Baisesレポート](https://app.wandb.ai/cayush/simpletransformers/reports/Using-simpleTransformer-on-common-NLP-applications---Vmlldzo4Njk2NA)を確認してください。colabで自分で試してみてください [![Open In Colab](https://camo.githubusercontent.com/52feade06f2fecbf006889a904d221e6a730c194/68747470733a2f2f636f6c61622e72657365617263682e676f6f676c652e636f6d2f6173736574732f636f6c61622d62616467652e737667)](https://colab.research.google.com/drive/1oXROllqMqVvBFcPgTKJRboTq96uWuqSz?usp=sharing)

