# Scikit Example

Here's a simple example project where we used wandb with sklearn. Try running [the example notebook](https://colab.research.google.com/drive/1tCppyqYFCeWsVVT4XHfck6thbhp3OGwZ) and see results on the [open project page](https://app.wandb.ai/wandb/iris).

![](../../.gitbook/assets/docs-example-colab-of-wandb-sklearn.png)

```python
import numpy as npfrom sklearn import datasetsfrom sklearn.preprocessing import StandardScalerfrom sklearn.model_selection import train_test_splitfrom sklearn.svm import SVCimport wandb# Initialize wandb# In this example we're sending runs to an open project I set up at# app.wandb.ai/wandb/iriswandb.init(entity="wandb", project="iris")# Set and save hyperparameters         wandb.config.gamma = 0.5wandb.config.C = 1.8wandb.config.test_size = 0.3wandb.config.seed = 0iris = datasets.load_iris()X = iris.data[:, [2, 3]]y = iris.targetX_train, X_test, y_train, y_test = train_test_split(X, y, test_size=wandb.config.test_size, random_state=wandb.config.seed)sc = StandardScaler()sc.fit(X_train)X_train_std = sc.transform(X_train)X_test_std = sc.transform(X_test)X_combined_std = np.vstack((X_train_std, X_test_std))y_combined = np.hstack((y_train, y_test))# Fit modelsvm = SVC(kernel='rbf', random_state=wandb.config.seed, gamma=wandb.config.gamma, C=wandb.config.C)svm.fit(X_train_std, y_train)# Save metricswandb.log({"Train Accuracy": svm.score(X_train_std, y_train),            "Test Accuracy": svm.score(X_test_std, y_test)})# Create a matplotlib custom plot to save def plot_data():    from matplotlib.colors import ListedColormap    import matplotlib.pyplot as plt    markers = ('s', 'x', 'o')    colors = ('red', 'blue', 'lightgreen')    cmap = ListedColormap(colors[:len(np.unique(y_test))])    for idx, cl in enumerate(np.unique(y)):        plt.scatter(x=X[y == cl, 0], y=X[y == cl, 1],               c=cmap(idx), marker=markers[idx], label=cl)    wandb.log({"Data": plt})plot_data()
```

