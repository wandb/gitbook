---
description: クラウドでホストされているサービスを使用する代わりに、検索と停止のアルゴリズムをローカルで実行します
---

# Local Controller

デフォルトでは、ハイパーパラメータコントローラーはW＆Bによってクラウドサービスとしてホストされます。W＆Bエージェントはコントローラーと通信して、トレーニングに使用する次のパラメーターセットを決定します。コントローラは、早期停止アルゴリズムを実行して、停止できる実行を決定する役割も果たします。

{% hint style="info" %}
The local controller is currently limited to running a single agent.
{% endhint %}

## **ローカルコントローラー構成**

ローカルコントローラを有効にするには、スイープ設定ファイルに以下を追加します。

```text
controller:
  type: local
```

## **ローカルコントローラーの実行**

次のコマンドは、スイープコントローラーを起動します。

```text
wandb controller SWEEP_ID
```

または、スイープを初期化するときにコントローラーを起動することもできます。

```text
wandb sweep --controller sweep.yaml
```

