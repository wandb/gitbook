# Scikit

If you are using scikit-learn, you can use wandb to track single experiments or metrics over cross validation or parameter searches.

```python
import wandb

# Initialize wandb
wandb.init(project="iris")
  
# set and save hyperparameters         
wandb.config.gamma = 0.1
wandb.config.C = 1.0
wandb.config.test_size = 0.3
wandb.config.seed = 0

svm = SVC(kernel='rbf', random_state=wandb.config.seed, gamma=wandb.config.gamma, \
C=wandb.config.C)
svm.fit(X_train_std, y_train)

# Save metrics
wandb.log({"Train Accuracy": svm.score(X_train_std, y_train),
           "Test Accuracy": svm.score(X_test_std, y_test)})
```

