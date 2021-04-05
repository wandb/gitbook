# Scikit

 ****你可以使用wandb来可视化和比较你的scikit-learn模型的性能，只需几行代码。[**看一个例子→**](https://colab.research.google.com/drive/1j_4UQTT0Lib8ueAU5zXECxesCj_ofjw7)**​**

### **绘制图**

#### **第1步：导入wandb并初始化一个新的运行。**

```python
import wandb
wandb.init(project="visualize-sklearn")

# load and preprocess dataset
# train a model
```

####  **第2步：可视化单个图表。**

```python
# Visualize single plot
wandb.sklearn.plot_confusion_matrix(y_true, y_pred, labels)
```

#### **或者一次性可视化全部图表。**

```python
# Visualize all classifier plots
wandb.sklearn.plot_classifier(clf, X_train, X_test, y_train, y_test, y_pred, y_probas, labels,
                                                         model_name='SVC', feature_names=None)

# All regression plots
wandb.sklearn.plot_regressor(reg, X_train, X_test, y_train, y_test,  model_name='Ridge')

# All clustering plots
wandb.sklearn.plot_clusterer(kmeans, X_train, cluster_labels, labels=None, model_name='KMeans')
```

### **支持的图表**

#### **学习曲线**

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.46.34-am.png)

在不同长度的数据集上训练模型，并生成训练集和测试集的交叉验证分数与数据集大小的关系图。

`wandb.sklearn.plot_learning_curve(model, X, y)`

* model（clf或reg）：传入一个拟合回归器或分类器
* X（数组型）：数据集特征
* y（数组型）：数据集标签。

#### ROC

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.48.02-am.png)

 ROC曲线绘制正例率（Y轴）和­假正例率（X轴）。理想分数为TPR = 1且FPR = 0，也就是左上角的那个点。通常我们计算ROC曲线下面积（AUC-ROC），AUC-ROC越大越好。

`wandb.sklearn.plot_roc(y_true, y_probas, labels)`

* y\_true（数组型）：测试组标签。
* y\_probas（数组型）：测试组预测概率。
* labels（列表型）：目标变量\(y\)的命名标签。

#### **类占比**

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.48.46-am.png)

绘制出训练组和测试组中目标类的分布图。有助于检测不平衡的类，并保证这些类不会对模型产生不相称的影响。

`wandb.sklearn.plot_class_proportions(y_train, y_test, ['dog', 'cat', 'owl'])`

* y\_train（数组型）：训练组标签。
* y\_test（数组型）：测试组标签。
* labels（列表型）：目标变量\(y\)的命名标签。

####  **精确率­召回率（PR）曲线**

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.48.17-am.png)

计算不同阈值的精确率和召回率之间的权衡。曲线下方面积大，代表着召回率和精确率都很高，高精确率代表低假正例率，高召回率代表低假负例率。

两者分数都高，说明分类器返回的结果比较准确（高精确率），以及返回全部正类结果中的绝大部分（高召回率）。当分类非常不平衡时，PR曲线非常有用。

`wandb.sklearn.plot_precision_recall(y_true, y_probas, labels)`

* y\_true（数组型）：测试组标签。
* y\_probas（数组型）：测试组预测概率
* labels（列表型）：目标变量\(y\)的命名标签。

#### **特征重要性**

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.48.31-am.png)

 评估并绘制分类任务中每项特征的重要性。仅适用于带有属性`featureimportances`的分类器，比如树。

`wandb.sklearn.plot_feature_importances(model, ['width', 'height, 'length'])`

* model \(clf\)：传入一个拟合分类器。
*  feature\_names（列表型）：特征的名称。把特征的索引值换成对应的名称，会使图表更容易读懂

#### **校准曲线**

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.49.00-am.png)

绘制分类器的预测概率被校准得如何以及如何校准未校准的分类器。比较基准逻辑回归模型、作为参数传入的模型以及其isotonic校准和sigmoid校准的估计预测概率。校准曲线离对角线越近越好。

转置的sigmoid样曲线代表过度拟合的分类器，而sigmoid样曲线代表欠拟合的分类器。通过训练模型的isotonic校准和sigmoid校准并比较其曲线，我们就可以判定模型是过度拟合还是欠拟合，确定以后，再判定哪种校准（isotonic还是sigmoid）有助于矫正。

更多细节，请查看[Sklearn的文档](https://scikit-learn.org/stable/auto_examples/calibration/plot_calibration_curve.html)。

`wandb.sklearn.plot_calibration_curve(clf, X, y, 'RandomForestClassifier')`

* model \(clf\)：传入一个拟合分类器。
* X （数组型）：训练集特征。
* y（数组型）：训练集标签。
* model\_name（字符串型）：模型名称。默认为“Classifier”。

####  **混淆矩阵（Confusion Matrix）**

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.49.11-am.png)

计算混淆矩阵来评估分类的准确率。可用来判定模型预测的质量、找到模型在预测中出错的模式。对角线代表着模型做对的预测，也就是说，在这条线上实际标签等于预测标签。

`wandb.sklearn.plot_confusion_matrix(y_true, y_pred, labels)`

* y\_true（数组型）：测试集标签。
* y\_pred（数组型）：测试集预测标签。
* labels（列表型）：目标变量\(y\)的命名标签。

#### **总结指标（Summary Metrics）**

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.49.28-am.png)

计算回归和分类算法的总结指标（如分类的F1、准确率、精确率和召回率，回归的均方误差（MSE）、平均绝对误差（MAE）、R2系数）。

`wandb.sklearn.plot_summary_metrics(model, X_train, X_test, y_train, y_test)`

* model \(clf或reg\)：传入一个拟合的回归器或分类器。
* X （数组型）：训练集特征。
* y（数组型）：训练集标签。
  * X\_test（数组型）：测试集特征。
* y\_test（数组型）：测试集标签。

####  **肘形图（Elbow Plot）**

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.52.21-am.png)

测量并绘制出方差的百分比，方差的百分比是聚类数和训练时间的函数。可用来挑选最优聚类数。

`wandb.sklearn.plot_elbow_curve(model, X_train)`

* model \(聚类器\): 传入一个拟合聚类器
* X \(数组\): 训练集特征.

#### **轮廓图**

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.53.12-am.png)

测量并绘制出一个聚类中每个点离相邻聚类中的点的接近程度。聚类的浓度与聚类的大小相对应。那条竖线代表着所有点的平均轮廓系数。

轮廓系数接近+1表示该样本远离相邻聚类。值为0表示该样本处于或非常接近两个相邻聚类的判定边界，负值表示这些样本可能被分配到了错误的聚类。

总而言之，我们希望全部轮廓聚类系数高于平均值（超过红线），尽可能接近1.我们还希望聚类的大小能够反映数据的底层模式。

`wandb.sklearn.plot_silhouette(model, X_train, ['spam', 'not spam'])`

* model \(聚类器\)：传入一个拟合聚类器（clusterer）
* X （数组型）：训练集特征。
  * cluster\_labels（列表型）：聚类标签名称。把聚类索引值换成对应的名称，使图表更容易读懂。

#### **离群值候选图（Outlier Candidates Plot）**

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.52.34-am.png)

通过库克距离测算数据点对回归模型的影响。影响严重倾斜的实例有可能是离群值。可用来检测离群值。

`wandb.sklearn.plot_outlier_candidates(model, X, y)`

* model（回归器）：传入一个拟合分类器。
* X （数组型）：训练集特征。
* y（数组型）：训练集标签。

####  **残差图**

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.52.46-am.png)

测量并绘制预测目标值（Y轴）与­真实目标值和预测目标值之差（x轴）的关系，以及残差的分布。

一般来说，拟合良好的模型的残差应该是随机分布的，因为好的模型可以解释数据集中的大部分现象，除了随机误差。

`wandb.sklearn.plot_residuals(model, X, y)`

* model（回归器）：传入一个拟合分类器。
* X （数组型）：训练集特征
* y（数组型）：训练集标签。

  如果你有什么问题，就发到我们的[Slack社区](https://app.slack.com/client/TL4V2PWQ3)吧，我们在那里回复。

## **示例**

* [在Colab运行](https://colab.research.google.com/drive/1tCppyqYFCeWsVVT4XHfck6thbhp3OGwZ)：从一个简单的笔记本入手
* [wandb仪](https://wandb.ai/wandb/iris)表盘：在W&B中查看结果。

