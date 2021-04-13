# Fast.ai

 ​如果你正在用**fastai**训练模型，利用`WandbCallback`很容易就集成到W&B。详细信息请查看带有示例的[交互式文档→](https://wandb.ai/borisd13/demo_config/reports/Visualize-track-compare-Fastai-models--Vmlldzo4MzAyNA)

首先安装Weights&Biases并登录：

```text
pip install wandbwandb login
```

 然后把回调函数（callback）添加到`learner`方法或`fit`方法：

```text
import wandbfrom fastai.callback.wandb import *​# start logging a wandb runwandb.init(project='my_project')​# To log only during one training phaselearn.fit(..., cbs=WandbCallback())​# To log continuously for all training phaseslearn = learner(..., cbs=WandbCallback())
```

如果你用的Fastai v1，请参考[Fastai v1文档](https://docs.wandb.ai/v/zh-hans/integrations/fast.ai/fastai-v1)。​

`WandbCallback` 接受下列参数：

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;</th>
      <th style="text-align:left">&#x8BF4;&#x660E;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">log</td>
      <td style="text-align:left">&quot;gradients&#x201D;&#xFF08;&#x9ED8;&#x8BA4;&#x503C;&#xFF09;&#x3001;&#x201C;parameters&#x201D;&#x3001;&#x201C;all&#x201D;&#x6216;&#x8005;None&#x3002;&#x635F;&#x5931;&#x548C;&#x6307;&#x6807;&#x603B;&#x662F;&#x4F1A;&#x88AB;&#x8BB0;&#x5F55;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left">log_preds</td>
      <td style="text-align:left">&#x662F;&#x5426;&#x8981;&#x8BB0;&#x5F55;&#x9884;&#x6D4B;&#x6837;&#x672C;&#xFF08;&#x9ED8;&#x8BA4;&#x4E3A;True&#xFF09;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left">log_model</td>
      <td style="text-align:left">&#x662F;&#x5426;&#x8981;&#x8BB0;&#x5F55;&#x6A21;&#x578B;&#xFF08;&#x9ED8;&#x8BA4;&#x4E3A;True&#xFF09;&#x3002;&#x8BE5;&#x53C2;&#x6570;&#x8FD8;&#x9700;&#x8981;SaveModelCallback</td>
    </tr>
    <tr>
      <td style="text-align:left">log_dataset</td>
      <td style="text-align:left">
        <ul>
          <li>False&#xFF08;&#x9ED8;&#x8BA4;&#x503C;&#xFF09;</li>
          <li>&#x4E3A;True&#x5C31;&#x4F1A;&#x8BB0;&#x5F55;learn.dls.path&#x6240;&#x5F15;&#x7528;&#x7684;&#x6587;&#x4EF6;&#x5939;</li>
          <li>&#x53EF;&#x663E;&#x5F0F;&#x6307;&#x5B9A;&#x4E00;&#x4E2A;&#x8DEF;&#x5F84;&#x6765;&#x5F15;&#x7528;&#x8981;&#x8BB0;&#x5F55;&#x7684;&#x6587;&#x4EF6;&#x5939;&#x3002;</li>
        </ul>
        <p>&#x6CE8;&#x610F;&#xFF1A;&#x5B50;&#x6587;&#x4EF6;&#x5939;&#x201C;models&#x201D;&#x59CB;&#x7EC8;&#x4F1A;&#x88AB;&#x5FFD;&#x7565;&#x3002;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">dataset_name</td>
      <td style="text-align:left">&#x6240;&#x8BB0;&#x5F55;&#x7684;&#x6570;&#x636E;&#x96C6;&#x7684;&#x540D;&#x79F0;&#xFF08;&#x9ED8;&#x8BA4;&#x4E3A;&#x6587;&#x4EF6;&#x5939;&#x540D;&#xFF09;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left">valid_dl</td>
      <td style="text-align:left"><code>DataLoaders</code>&#x5305;&#x542B;&#x4E86;&#x9884;&#x6D4B;&#x6837;&#x672C;&#x7528;&#x5230;&#x7684;&#x9879;&#xFF08;&#x9ED8;&#x8BA4;&#x4E3A;&#x6765;&#x81EA;<code>learn.dls.valid</code>&#x7684;&#x968F;&#x673A;&#x9879;&#x3002;&#xFF09;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left">n_preds</td>
      <td style="text-align:left">&#x8BB0;&#x5F55;&#x7684;&#x9884;&#x6D4B;&#x6570;&#x91CF;&#xFF08;&#x9ED8;&#x8BA4;&#x4E3A;36&#xFF09;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left">seed</td>
      <td style="text-align:left">&#x7528;&#x4E8E;&#x6307;&#x5B9A;&#x968F;&#x673A;&#x6837;&#x672C;&#x3002;</td>
    </tr>
  </tbody>
</table>

사용자 정의 워크 플로우의 경우, 수동으로 데이터 세트 및 모델을 로그할 수 있습니다:

* `log_dataset(path, name=None, medata={})`
* `log_model(path, name=None, metadata={})`

对于自定义工作流，你可以手动记录数据集和模型：

注意：任何子文件夹“models”会被忽略。

## **示例** <a id="examples"></a>

* ​[可视化、跟踪与比较Fastai模型](https://wandb.ai/borisd13/demo_config/reports/Visualize-track-compare-Fastai-models--Vmlldzo4MzAyNA)：详尽教程
* [CamVid上的图像分割：](https://colab.research.google.com/drive/1IWrhwcJoncCKHm6VXsNwOr9Yukhz3B49?usp=sharing)一个集成的使用案例 

