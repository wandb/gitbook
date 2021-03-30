---
description: 如何使用Weights&Biases的一些项目示例、演练和教程。
---

# Examples

 探索这些示例如何使用Weights&Biases以：

* 跟踪和可视化机器学习实验；
* 版本化数据集和模型
* 使用不同框架如PyTorch、Sciki监测（Instrument）模型

可以从我们的[GitHub仓](https://github.com/wandb/examples)库Fork示例，也可以直接在这里打开示例链接。如果要向列表贡献示例，请联系我们c@wandb.com

### **W&B入门**

| 描述 | 仪表盘 | 代码 |
| :--- | :--- | :--- |
| 跟踪模型性能 | [W&B链接](https://wandb.ai/lavanyashukla/visualize-models/reports/Track-Model-Performance--Vmlldzo1NTk2MA)​ | ​[Kaggle内](https://www.kaggle.com/lavanyashukla01/better-models-faster-with-weights-biases)核​ ​ |
| 可视化模型预测 | [W&B链接](https://wandb.ai/lavanyashukla/visualize-predictions/reports/Visualize-Model-Predictions--Vmlldzo1NjM4OA)​ | ​[Kaggle内](https://www.kaggle.com/lavanyashukla01/visualizing-model-performance-with-w-b)核​ |
| 保存和恢复模型 |  [W&B链接](https://wandb.ai/lavanyashukla/save_and_restore/reports/Saving-and-Restoring-Models-with-W&B--Vmlldzo3MDQ3Mw)​ | [ ](https://colab.research.google.com/drive/1pVlV6Ua4C695jVbLoG-wtc50wZ9OOjnC?authuser=1#scrollTo=0LB6j3O-jIsd)[Colab笔记本](https://colab.research.google.com/drive/1pVlV6Ua4C695jVbLoG-wtc50wZ9OOjnC?authuser=1#scrollTo=0LB6j3O-jIsd) |
| 比较系统指标（Metric） | [W&B链接](https://wandb.ai/stacey/estuary/reports/System-metrics-for-model-comparison--Vmlldzo1NzI5Mg)​ |  |
| 超参数扫描（Sweeps） |  [W&B链接](https://wandb.ai/sweep/sweeps-tutorial/workspace?workspace=user-lavanyashukla)​ | [Colab笔记本](https://colab.research.google.com/drive/1gKixa6hNUB8qrn1CfHirOfTEQm0qLCSS?pli=1&authuser=1) |

### PyTorch

| 描述 | 仪表盘 | 代码 |
| :--- | :--- | :--- |
| 带W&B的PyTorch简介 |  [W&B仪表盘](https://app.wandb.ai/wandb/pytorch-intro)​ |  [Colab笔记本](https://github.com/wandb/examples/blob/master/examples/pytorch/pytorch-intro/intro.ipynb) |
| PyTorch MNIST Colab |  [W&B仪表盘](https://app.wandb.ai/wandb/pytorch-mnist)​ |  [Colab笔记本](https://colab.research.google.com/drive/1zkoPdBZWUMsTpvA35ShVNAP0QcRsPUjf?pli=1&authuser=1#scrollTo=oewlztOSe5n4) |
| 着色卷积神经网络（CNN）将灰度图转换为彩图 | [W&B仪表盘](https://app.wandb.ai/clarence-n-huang/color-best-looking/reports?view=carey%2FColorizing%20Images) | [Github仓](https://github.com/clarencenhuang/dl-colorize)库​ |
| Yolo-2边界框 | [W&B仪表盘](https://app.wandb.ai/l2k2/darknet)​ | [Github 仓](https://github.com/lukas/pytorch-yolo2)库​ |
| 强化学习 | [ ](https://wandb.ai/kairproject/kair_algorithms_draft-scripts/runs/ylmssdkf)[W&B仪](https://wandb.ai/kairproject/kair_algorithms_draft-scripts/runs/ylmssdkf)表盘​ | [Github仓](https://github.com/kairproject/kair_algorithms_draft)库​ |
| char-RNN预测文字 |  [W&B仪](https://wandb.ai/borisd13/char-RNN)表盘​ | [Github 仓](https://github.com/borisdayma/char-RNN)库​ |
| 利用权阈探究ResNets |  [W&B仪](https://wandb.ai/cayush/resnet/reports/Exploring-ResNets-With-W&B--Vmlldzo2NDc4NA)表盘​ |  [Colab笔记本](https://colab.research.google.com/drive/1s62r_nK4RNd3PIyrAd2H72gvrMElX3hN?usp=sharing&pli=1&authuser=1)​ |
| 利用权阈探究神经风格迁移论文 |  [W&B仪](https://wandb.ai/cayush/resnet/reports/Exploring-ResNets-With-W&B--Vmlldzo2NDc4NA)表盘​ | [Github 仓](https://github.com/AyushExel/Neural-Style-Transfer)库​ |
| 用PyTorch调试神经网络 | [W&B报告](https://wandb.ai/ayush-thakur/debug-neural-nets/reports/Visualizing-and-Debugging-Neural-Networks-with-PyTorch-and-W-B--Vmlldzo2OTUzNA)​ | [GitHub 仓](https://github.com/ayulockin/debugNNwithWandB)库​ |
| PyTorch Lightning |  [W&B报告](https://wandb.ai/cayush/pytorchlightning/reports/Use-Pytorch-Lightning-with-Weights-Biases--Vmlldzo2NjQ1Mw)​ |  [Colab笔记本](https://colab.research.google.com/drive/1GHWwfzAsWx_Q1paw73hngAvA7-U9QHi-?pli=1&authuser=1) |
| 用PyTorch Lightning做语义分割 | [W&B仪](https://wandb.ai/borisd13/lightning-kitti/reports/Lightning-Kitti--Vmlldzo3MTcyMw)表盘​ | [Github 仓](https://github.com/borisdayma/lightning-kitti)库​ |

### Keras

| 描述 | 仪表盘 | 代码 |
| :--- | :--- | :--- |
| 带W&B的Keras简介 |  [W&B仪](https://wandb.ai/wandb/keras-intro)表盘​ |  [Colab笔记本](https://colab.research.google.com/drive/1pMcNYctQpRoBKD5Z0iXeFWQD8hIDgzCV?pli=1&authuser=1) |
| 带W&B的卷积神经网络（CNN）简介 |  [W&B仪](https://wandb.ai/wandb/cnn-intro?workspace=)表盘​ |  [Colab笔记本](https://colab.research.google.com/drive/1S8SJvH4bqhPvurG4gjh3-t-XulX4S8JX?pli=1&authuser=1) |
| 着色卷积神经网络CNN将灰度图转化为彩图 |  [W&B仪](https://wandb.ai/borisd13/colorizer/reports?view=carey%2FColorizing%20Black%20and%20White%20Images)表盘​ | [Github 仓](https://github.com/borisd13/colorizer)库​ |
| 卷积神经网络（CNN）人脸表情分类器 |  [W&B仪](https://wandb.ai/wandb/face-emotion)表盘​ | [Github 仓](https://github.com/lukas/face_classification)库​ |
| 用Mask RCNN做语义分割 | [W&B仪](https://wandb.ai/trentwatson1/mask-rcnn/?workspace=user-lavanyashukla)表盘​ | [Github 仓](https://github.com/connorhough/mask_rcnn)库​ |
| 在iNaturalist数据上微调CNN |   [W&B仪](https://wandb.ai/stacey/keras_finetune?workspace=user-l2k2)表盘​ | [Github 仓](https://github.com/wandb/examples/tree/master/examples/keras/keras-cnn-nature)库​ |
| 用U-Net做语义分割 |  [W&B仪](https://wandb.ai/gabesmed/witness)表盘​ | [Github 仓](https://github.com/wandb/witness)库​ |
| 权重（Weight）初始化对神经网络的影响 |  [W&B仪](https://wandb.ai/sayakpaul/weight-initialization-tb/reports/Effects-of-Weight-Initialization-on-Neural-Networks--Vmlldzo2ODY0NA)表盘​ |  [Colab笔记本](https://colab.research.google.com/drive/1Faqy6QaOkG-5G31MrYmvcmm079XbfKSv?pli=1&authuser=1) |
|  神经图像生成器可以被检测到吗？ |  [W&B仪](https://wandb.ai/lavanyashukla/cnndetection/reports/Can-Neural-Image-Generators-Be-Detected---Vmlldzo2MTU1Mw)表盘​ |  |
| 可视化模型预测 | [W&B仪](https://wandb.ai/lavanyashukla/visualize-predictions/reports/Visualize-Model-Predictions--Vmlldzo1NjM4OA)表盘​ | [Kaggle 内](https://www.kaggle.com/lavanyashukla01/visualizing-model-performance-with-w-b)核​ |
| 跟踪模型性能 | [W&B仪](https://wandb.ai/lavanyashukla/visualize-models/reports/Track-Model-Performance--Vmlldzo1NTk2MA)表盘​ | [Kaggle 内](https://www.kaggle.com/lavanyashukla01/better-models-faster-with-weights-biases)核​ |
| 在TensorBoard中用权重（Weight）&偏差（Bias）可视化模型 | [W&B仪](https://wandb.ai/lavanyashukla/visualize-models/reports/Track-Model-Performance--Vmlldzo1NTk2MA)表盘​ |  [Colab笔记本](https://colab.research.google.com/gist/sayakpaul/5b31ed03725cc6ae2af41848d4acee45/demo_tensorboard.ipynb) |
| 使用TPU的权重\(（Weights）&偏差（Biases） |  |  [Colab笔记本](https://colab.research.google.com/drive/1gXEr0a_8ZbHt5-uO80JdQJxJ_uoYR4qv?usp=sharing&pli=1&authuser=1) |

### TensorFlow

| 描述 | 仪表盘 | 代码 |
| :--- | :--- | :--- |
| 用GAN预测视频帧 | [W&B仪](https://wandb.ai/wandb/catz/runs/qfsbxd3r?workspace=user-)表盘​ | [Github 仓](https://github.com/sirebellum/catz_contest)库​ |
| 跟踪TensorFlow模型性能 |  | ​[Github 仓](https://github.com/wandb/examples/blob/master/examples/tensorflow/tf-estimator-mnist/mnist.py)库​ |
| TensorFlow分布式训练模型 |  | [Github 仓](https://github.com/wandb/examples/tree/master/examples/tensorflow/tf-distributed-mnist/train.py)库​ |

### Fast.ai

| 描述 | 仪表盘 | 代码 |
| :--- | :--- | :--- |
| 通过fastai使 | [W&B仪表盘](https://wandb.ai/borisd13/demo_config/reports/Visualize-Track-Compare-Fastai-Models--Vmlldzo4MzAyNA)​ | [Github Repo](https://github.com/borisdayma/simpsons-fastai) |
| 用WandbCallback | [ 权阈指示板](https://wandb.ai/borisd13/semantic-segmentation/?workspace=user-borisd13) | [Github Repo](https://github.com/borisdayma/semantic-segmentation/blob/master/src/train.py) |

 **扫描（**Sweeps**）**

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x63CF;&#x8FF0;</th>
      <th style="text-align:left">&#x516C;&#x5171;&#x4EEA;&#x8868;&#x76D8;</th>
      <th style="text-align:left">&#x4EE3;&#x7801;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p>&#x5E26;&#x6709;W&amp;B&#x7684;&#x626B;&#x63CF;&#xFF08;Sweeps</p>
        <p>&#xFF09;&#x7B80;&#x4ECB;</p>
      </td>
      <td style="text-align:left"> <a href="https://wandb.ai/sweep/simpsons?workspace=user-lavanyashukla">W&amp;B&#x4EEA;</a>&#x8868;&#x76D8;&#x200B;</td>
      <td
      style="text-align:left"><a href="https://colab.research.google.com/drive/181GCGp36_75C2zm7WLxr9U2QjMXXoibt?pli=1&amp;authuser=1">Colab&#x7B14;&#x8BB0;&#x672C;</a>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">PyTorch&#x626B;&#x63CF;&#xFF1A;&#x8D85;&#x53C2;&#x6570;&#x641C;&#x7D22;&#x7684;&#x610F;&#x4E49;&#x548C;&#x566A;&#x58F0;</td>
      <td
      style="text-align:left"><a href="https://wandb.ai/stacey/pytorch_intro/reports/Meaning-and-Noise-in-Hyperparameter-Search--Vmlldzo0Mzk5MQ">&#x62A5;&#x544A;</a>
        </td>
        <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">&#x5728;Python&#x811A;&#x672C;&#x4E2D;&#x8FD0;&#x884C;&#x626B;&#x63CF;</td>
      <td
      style="text-align:left"></td>
        <td style="text-align:left"><a href="https://github.com/wandb/examples/blob/master/examples/wandb-sweeps/sweeps-python/sweep.py">Github &#x4ED3;</a>&#x5E93;&#x200B;</td>
    </tr>
    <tr>
      <td style="text-align:left">&#x5728;MPI&#x6846;&#x67B6;&#x4E2D;&#x4F7F;&#x7528;&#x626B;&#x63CF;</td>
      <td
      style="text-align:left"></td>
        <td style="text-align:left"><a href="https://github.com/wandb/examples/tree/master/examples/wandb-sweeps/sweeps-mpi-wrappers">Github &#x4ED3;</a>&#x5E93;&#x200B;</td>
    </tr>
  </tbody>
</table>

### Scikit-learn

| 描述 | 公共仪表盘 | 代码 |
| :--- | :--- | :--- |
| 可视化Scikit模型 | ​[W&B仪](https://wandb.ai/lavanyashukla/visualize-sklearn/reports/Visualize-Scikit-Models--Vmlldzo0ODIzNg)表盘 | [Colab笔记本](https://colab.research.google.com/drive/1j_4UQTT0Lib8ueAU5zXECxesCj_ofjw7?pli=1&authuser=1) |
| 在XGBoost中使用W&B |  | [Github 仓](https://github.com/wandb/examples/tree/master/examples/boosting-algorithms/xgboost-dermatology)库​ |
| 在一个SVM中使用W&B |  | ​[Github 仓](https://github.com/wandb/examples/tree/master/examples/scikit/scikit-iris)库​ |

### **应用**

| 标题 | 链接 |
| :--- | :--- |
| 蛋白结构预测 |  [报告](https://wandb.ai/koes-group/protein-transformer/reports/Evaluating-the-Impact-of-Sequence-Convolutions-and-Embeddings-on-Protein-Structure-Prediction--Vmlldzo2OTg4Nw) |
| 语义分割在自动驾驶汽车中的应用 |  [报告](https://wandb.ai/stacey/deep-drive/reports/The-View-from-the-Driver%27s-Seat--Vmlldzo1MTg5NQ) |
| 新冠肺炎研究范例 |  [报告](https://wandb.ai/cayush/covid-19-scans/reports/COVID-19-research-using-PyTorch-and-W&B--Vmlldzo2OTQ5OA) |
| 从视频中获取深度图在自动驾驶汽车中的应用 |  [报告](https://wandb.ai/stacey/sfmlearner/reports/See-3D-from-Video:-Depth-Perception-for-Self-Driving-Cars--Vmlldzo2Nzg2Nw) |
| 权重（Weight）初始化对神经网络的影响 |  [报告](https://wandb.ai/sayakpaul/weight-initialization-tb/reports/Effects-of-Weight-Initialization-on-Neural-Networks--Vmlldzo2ODY0NA) |

 **功能**

| 标题 | 链接 |
| :--- | :--- |
| 集成抱抱脸（HuggingFace） |  [Colab笔记本](https://colab.research.google.com/drive/1NEiqNPhiouu2pPwDAVeFoN4-vTYMz9F8?pli=1&authuser=1) |



