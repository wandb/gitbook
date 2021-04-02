# SimpleTransformers

这个库是基于抱抱脸（HuggingFace）的Transformers库。SimpleTransformers 可以让你快速训练和评估Transformer模型。只需3行代码就可以初始化模型、训练模型和评估模型。它支持Sequence Classification, Token Classification \(NER\),Question Answering,Language Model Fine-Tuning, Language Model Training, Language Generation, T5 Model, Seq2Seq Tasks , Multi-Modal Classification 和Conversational AI.

## **Weights & Biases框架** <a id="the-weights-and-biases-framework"></a>

支持Weights和Biases以实现模型训练的可视化。要使用这个功能，只需在 `args`字典的`wandb_project`属性中为W&B设置一个项目名称。这将把所有超参数值、训练损失和评估指标记录到给定项目中。

```text
model = ClassificationModel('roberta', 'roberta-base', args={'wandb_project': 'project-name'})
```

任何传入`wandb.init`的附加参数都可以作为`wandb_kwargs`参数传递。 

## **架构** <a id="structure"></a>

 该库被设计为为每个NLP任务设计一个单独的类。提供类似功能的类被分组到一起。

* `simpletransformers.classification` - 包括所有分类模型。
  * `ClassificationModel`
  * `MultiLabelClassificationModel`
* `simpletransformers.ner` - 包括所有分类模型。
  * `NERModel`
* `simpletransformers.question_answering` - 包括所有命名实体识别（Named Entity Recognition）模型。
  * `QuestionAnsweringModel`

这里有一些简单的例子

## MultiLabel Classification <a id="multilabel-classification"></a>

```text
  model = MultiLabelClassificationModel("distilbert","distilbert-base-uncased",num_labels=6,    args={"reprocess_input_data": True, "overwrite_output_dir": True, "num_train_epochs":epochs,'learning_rate':learning_rate,                'wandb_project': "simpletransformers"},  )   # Train the model  model.train_model(train_df)​  # Evaluate the model  result, model_outputs, wrong_predictions = model.eval_model(eval_df)
```

下面是上面的训练脚本在运行超参数扫描后生成的一些可视化结果。​[​](https://camo.githubusercontent.com/3beab1ca06813523711ff7750cb592430b786834/68747470733a2f2f692e696d6775722e636f6d2f6f63784e676c642e706e67)

​[​![](https://camo.githubusercontent.com/3beab1ca06813523711ff7750cb592430b786834/68747470733a2f2f692e696d6775722e636f6d2f6f63784e676c642e706e67)​](https://camo.githubusercontent.com/3beab1ca06813523711ff7750cb592430b786834/68747470733a2f2f692e696d6775722e636f6d2f6f63784e676c642e706e67)​

​[​![](https://camo.githubusercontent.com/b864ca220ddd4228027743790ac30741d1f435ad/68747470733a2f2f692e696d6775722e636f6d2f5252423432374d2e706e67)​](https://camo.githubusercontent.com/b864ca220ddd4228027743790ac30741d1f435ad/68747470733a2f2f692e696d6775722e636f6d2f5252423432374d2e706e67)​

## Question Answering <a id="question-answering"></a>

```text
  train_args = {    'learning_rate': wandb.config.learning_rate,    'num_train_epochs': 2,    'max_seq_length': 128,    'doc_stride': 64,    'overwrite_output_dir': True,    'reprocess_input_data': False,    'train_batch_size': 2,    'fp16': False,    'wandb_project': "simpletransformers"}​model = QuestionAnsweringModel('distilbert', 'distilbert-base-cased', args=train_args)model.train_model(train_data)
```

下面是上面的训练脚本在运行超参数扫描后生成的一些可视化结果。​[​](https://camo.githubusercontent.com/1411cacec6226ebfa23c2e2dddc76ff5e41c136d/68747470733a2f2f692e696d6775722e636f6d2f7664636d7855532e706e67)

​[​![](https://camo.githubusercontent.com/1411cacec6226ebfa23c2e2dddc76ff5e41c136d/68747470733a2f2f692e696d6775722e636f6d2f7664636d7855532e706e67)​](https://camo.githubusercontent.com/1411cacec6226ebfa23c2e2dddc76ff5e41c136d/68747470733a2f2f692e696d6775722e636f6d2f7664636d7855532e706e67)​

​[​![](https://camo.githubusercontent.com/b8e12316520d4ad6d16449db2d13ab70e4d4a6e9/68747470733a2f2f692e696d6775722e636f6d2f395732775677732e706e67)​](https://camo.githubusercontent.com/b8e12316520d4ad6d16449db2d13ab70e4d4a6e9/68747470733a2f2f692e696d6775722e636f6d2f395732775677732e706e67)​

SimpleTransformers为所有常见的自然语言任务提供了类和训练脚本。以下该库支持的全局参数的完整列表，以及他们的默认参数。

```text
global_args = {  "adam_epsilon": 1e-8,  "best_model_dir": "outputs/best_model",  "cache_dir": "cache_dir/",  "config": {},  "do_lower_case": False,  "early_stopping_consider_epochs": False,  "early_stopping_delta": 0,  "early_stopping_metric": "eval_loss",  "early_stopping_metric_minimize": True,  "early_stopping_patience": 3,  "encoding": None,  "eval_batch_size": 8,  "evaluate_during_training": False,  "evaluate_during_training_silent": True,  "evaluate_during_training_steps": 2000,  "evaluate_during_training_verbose": False,  "fp16": True,  "fp16_opt_level": "O1",  "gradient_accumulation_steps": 1,  "learning_rate": 4e-5,  "local_rank": -1,  "logging_steps": 50,  "manual_seed": None,  "max_grad_norm": 1.0,  "max_seq_length": 128,  "multiprocessing_chunksize": 500,  "n_gpu": 1,  "no_cache": False,  "no_save": False,  "num_train_epochs": 1,  "output_dir": "outputs/",  "overwrite_output_dir": False,  "process_count": cpu_count() - 2 if cpu_count() > 2 else 1,  "reprocess_input_data": True,  "save_best_model": True,  "save_eval_checkpoints": True,  "save_model_every_epoch": True,  "save_steps": 2000,  "save_optimizer_and_scheduler": True,  "silent": False,  "tensorboard_dir": None,  "train_batch_size": 8,  "use_cached_eval_features": False,  "use_early_stopping": False,  "use_multiprocessing": True,  "wandb_kwargs": {},  "wandb_project": None,  "warmup_ratio": 0.06,  "warmup_steps": 0,  "weight_decay": 0,}
```

更多详细的文档请参考[ github](https://github.com/ThilinaRajapakse/simpletransformers)上的simpletransformers

查看[这份Weights和Baises报告](https://app.wandb.ai/cayush/simpletransformers/reports/Using-simpleTransformer-on-common-NLP-applications---Vmlldzo4Njk2NA)that，它涵盖了在一些最流行的的GLUE基准数据集上训练transformers。 自己在[​![Open In Colab](https://camo.githubusercontent.com/52feade06f2fecbf006889a904d221e6a730c194/68747470733a2f2f636f6c61622e72657365617263682e676f6f676c652e636f6d2f6173736574732f636f6c61622d62616467652e737667)](https://colab.research.google.com/drive/1oXROllqMqVvBFcPgTKJRboTq96uWuqSz?usp=sharing)上试试吧。 [​](https://colab.research.google.com/drive/1oXROllqMqVvBFcPgTKJRboTq96uWuqSz?usp=sharing)​

