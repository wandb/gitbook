---
description: wandbによって自動的にログに記録されるメトリック
---

# System Metrics

wandbは、30秒間の平均で、2秒ごとにシステムメトリックを自動的にログに記録します。指標は次のとおりです。

* CPU使用率
*  システムメモリの使用率
*  ディスクI/Oの使用率
* ネットワークトラフィック（送受信されたバイト数）
* GPUの使用率
* GPU温度
* メモリへのアクセスに費やされたGPU時間（サンプル時間のパーセンテージとして）
* 割り当てられたGPUメモリ

メトリックを解釈してモデルのパフォーマンスを最適化する方法の詳細については、[Lambda Labsのこの役立つブログ投稿を](https://lambdalabs.com/blog/weights-and-bias-gpu-cpu-utilization/)参照してください。

