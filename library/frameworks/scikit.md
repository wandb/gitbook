# Scikit

If you are using scikit-learn, you can use wandb to track single experiments or metrics over cross validation or parameter searches.

```python
import wandbfrom sklearn.svm import SVCfrom sklearn import datasetsfrom sklearn.model_selection import train_test_split# Initialize wandbwandb.init(project="iris")# set and save hyperparameters         wandb.config.gamma = 0.1wandb.config.C = 1.0wandb.config.test_size = 0.3wandb.config.seed = 0# import iris datasetiris = datasets.load_iris()X_train, X_test, y_train, y_test = train_test_split(    iris.data, iris.target, test_size=wandb.config.test_size,     random_state=wandb.config.seed)# fit modelsvm = SVC(kernel='rbf', random_state=wandb.config.seed, gamma=wandb.config.gamma, \C=wandb.config.C)svm.fit(X_train, y_train)# Save metricswandb.log({"Train Accuracy": svm.score(X_train, y_train),           "Test Accuracy": svm.score(X_test, y_test)})
```

