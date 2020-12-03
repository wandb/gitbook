---
description: wandb.keras
---

# Keras Reference

[source](https://github.com/wandb/client/blob/master/wandb/keras/__init__.py#L148)

```python
WandbCallback(self,
              monitor='val_loss',
              verbose=0,
              mode='auto',
              save_weights_only=False,
              log_weights=False,
              log_gradients=False,
              save_model=True,
              training_data=None,
              validation_data=None,
              labels=[],
              data_type=None,
              predictions=36,
              generator=None,
              input_type=None,
              output_type=None,
              log_evaluation=False,
              validation_steps=None,
              class_colors=None,
              log_batch_frequency=None,
              log_best_prefix='best_')
```

WandbCallback automatically integrates keras with wandb.

**例：**

```python
model.fit(X_train, y_train,  validation_data=(X_test, y_test),
callbacks=[WandbCallback()])
```

WandbCallbackは、kerasによって収集されたすべてのメトリック（ロスおよびkeras\_model.compile（）に渡されたもの）から履歴データを自動的にログに記録します

WandbCallbackは、「最良の」トレーニングステップに関連付けられた実行の要約メトリックを設定します。ここで、「最良」はモニターとモードの属性によって定義されます。これはデフォルトで、val\_lossが最小のエポックになります。WandbCallbackは、デフォルトで、最良のエポックに関連付けられたモデルを保存します。

WandbCallbackは、オプションで勾配とパラメーターのヒストグラムをログに記録できます。

WandbCallbackは、オプションで、wandbが視覚化するためのトレーニングおよび検証データを保存できます。



**主張：**

* monitor str‐監視するメトリックの名前。デフォルトはval\_lossです。
* mode str‐{"auto", "min", "max"}のいずれか。「min」-モニターが最小化されたときにモデルを保存します「max」-モニターが最大化されたときにモデルを保存します「auto」‐モデルを保存するタイミングを推測してみます（デフォルト）。save\_model：True‐モニターが以前のすべてのエポックを上回ったときにモデルを保存しますFalse-モデルを保存しません
* save\_weights\_only boolean‐Trueの場合、モデルの重みのみが保存されます（\(model.save\_weights\(filepath\)\)。それ以外の場合は、モデル全体が保存されます（\(model.save\(filepath\)\)。
* og\_weights‐（ブール値）Trueの場合、モデルのレイヤーの重みのヒストグラムを保存します。
* log\_gradients‐（ブール値）トレーニング勾配の真の対数ヒストグラムの場合。モデルはtotal\_lossを定義する必要があります。
* training\_data‐（タプル）model.fitに渡されるのと同じ形式（X、y）。これは勾配を計算するために必要です‐log\_gradientsがTrueの場合、これは必須です。
* validation\_data‐（タプル）model.fitに渡されるのと同じ形式（X、y）。wandbが視覚化するためのデータセット。これが設定されている場合、すべてのエポック、wandbは少数の予測を行い、後で視覚化するために結果を保存します。
* generator generator‐wandbが視覚化するための検証データを返すジェネレーター。このジェネレーターはタプル（X、y）を返す必要があります。特定のデータ例を視覚化するには、validate\_dataまたはgeneratorのいずれかをwandbに設定する必要があります。
* validation\_steps int‐validation\_dataがジェネレーターの場合、完全な検証セットに対してジェネレーターを実行するためのステップ数。
* labels list‐wandbを使用してデータを視覚化している場合、マルチクラス分類子を構築している場合、このラベルリストは数値出力を理解可能な文字列に変換します。バイナリ分類子を作成している場合は、2つのラベルのリストを渡すことができます\["label for false", "label for true"\]。validate\_dataとgeneratorの両方がfalseの場合、これは何もしません。
* predictions int‐各エポックを視覚化するために行う予測の数。最大は100です。
* input\_type string‐視覚化に役立つモデル入力のタイプ。\("image", "images", "segmentation\_mask"\)のいずれかになります。
* output\_type string‐視覚化に役立つモデル出力のタイプ。\("image", "images", "segmentation\_mask"\)のいずれかになります。
* log\_evaluation boolean‐Trueの場合、トレーニングの最後に完全な検証結果を含むデータフレームを保存します。
* class\_colors \[float, float, float\] ‐入力または出力がセグメンテーションマスクの場合、各クラスのrgbタプル（範囲0-1）を含む配列。
* log\_batch\_frequency integer‐Noneの場合、コールバックはすべてのエポックをログに記録します。整数に設定されている場合、コールバックはlog\_batch\_frequencyバッチごとにトレーニングメトリックをログに記録します。
* log\_best\_prefix string‐Noneの場合、追加のサマリーメトリックは保存されません。文字列に設定すると、監視対象のメトリックとエポックにこの値が付加され、サマリーメトリックとして保存されます。

