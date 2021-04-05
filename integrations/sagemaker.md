# SageMaker

##  **SageMakerの統合**

 W＆Bは[Amazon SageMaker](https://aws.amazon.com/sagemaker/)と統合され、ハイパーパラメータを自動的に読み取り、分散実行をグループ化し、チェックポイントから実行を再開します。

###  認証

W＆Bは、トレーニングスクリプトに関連する`secrets.env`という名前のファイルを探し、`wandb.init（）`が呼び出されたときにそれらを環境にロードします。実験の起動に使用するスクリプトで`wandb.sagemaker_auth（path = "source_dir"）`を呼び出すことにより、secrets.envファイルを生成できます。このファイルを`.gitignore`に必ず追加してください。

### 既存の推定量

SageMakersの事前設定されたエスティメータのいずれかを使用している場合は、wandbを含むソースディレクトリにrequirements.txtを追加する必要があります

```text
wandb
```

Python 2を実行しているEstimatorを使用している場合は、wandbをインストールする前に、[ホイール](https://pythonwheels.com/)から直接psutilをインストールする必要があります。

|  |
| :--- |


```text
https://wheels.galaxyproject.org/packages/psutil-5.4.8-cp27-cp27mu-manylinux1_x86_64.whl
wandb
```

 完全な例は[GitHub](https://github.com/wandb/examples/tree/master/examples/pytorch/pytorch-cifar10-sagemaker)で入手でき、[ブログ](https://www.wandb.com/articles/running-sweeps-with-sagemaker)で詳細を読むことができます。

