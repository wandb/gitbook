# Visualize Sweep Results

## Parallel coordinates plot

![](https://paper-attachments.dropbox.com/s_194708415DEC35F74A7691FF6810D3B14703D1EFE1672ED29000BA98171242A5_1578695138341_image.png)

Parallel coordinates plots map hyperparameter values to model metrics. They're useful for honing in on combinations of \_\*\*\_hyperparameters that led to the best model performance.

## Hyperparameter Importance Plot

![](https://paper-attachments.dropbox.com/s_194708415DEC35F74A7691FF6810D3B14703D1EFE1672ED29000BA98171242A5_1578695757573_image.png)

The hyperparameter importance plot surfaces which hyperparameters were the best predictors of, and highly correlated to desirable values for your metrics.

**Correlation** is the linear correlation between the hyperparameter and the chosen metric \(in this case val\_loss\). So a high correlation means that when the hyperparameter has a higher value, the metric also has higher values and vice versa. Correlation is a great metric to look at but it can’t capture second order interactions between inputs and it can get messy to compare inputs with wildly different ranges.

Therefore we also calculate an **importance** metric where we train a random forest with the hyperparameters as inputs and the metric as the target output and report the feature importance values for the random forest.

