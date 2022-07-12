---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.2'
      jupytext_version: 1.5.1
  kernelspec:
    display_name: Python 3
    language: python
    name: python3
---

```python
import os
from typing import Tuple, Union

import mlflow
import mlflow.sklearn
import numpy as np
import pandas as pd
from sklearn.datasets import load_boston
from sklearn.linear_model import LinearRegression, ElasticNet
from sklearn.metrics import mean_squared_error, mean_absolute_error, r2_score
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeRegressor
```

# Prepare dataset

<!-- #region pycharm={"name": "#%% md\n"} -->
A dataset is loaded and divided into training and test part.
<!-- #endregion -->

```python pycharm={"name": "#%%\n"}
# Load dataset
X, y = load_boston(return_X_y=True)
# Split the dataset into training and test parts. (0.75, 0.25) split.
train_X, test_X, train_y, test_y = train_test_split(X, y, random_state=42)
```

<!-- #region pycharm={"name": "#%% md\n"} -->
# Experiment
<!-- #endregion -->

## Set MLflow experiment name


By calling mlflow.set_experiment function set the experiment name to be equal to combination of the project name and a task name.

```python
mlflow.set_experiment(f"{os.environ['PROJECT_NAME']}/custom_task_name")
```

## Helper functions

<!-- #region pycharm={"name": "#%% md\n"} -->
To simplify evaluation the compute_metrics function can be used to compute metrics that will be logged.
<!-- #endregion -->

```python pycharm={"name": "#%%\n"}
def compute_metrics(
    actual: Union[np.ndarray, pd.DataFrame, pd.Series],
    predicted: Union[np.ndarray, pd.DataFrame, pd.Series]
) -> Tuple[float, float, float]:
    """Computes RMSE, MAE and R2 evaluation metrics.

    Parameters
    ----------
    actual : array-like
        Ground truth values.
    predicted : array-like
        Predicted values.

    Returns
    -------
    Tuple[float, float, float]
        A tuple containg in order RMSE (Root Mean Square Error), MAE (Mean Absolute Error),
        R2 (coefficient of determination).
    """
    rmse = np.sqrt(mean_squared_error(actual, predicted))
    mae = mean_absolute_error(actual, predicted)
    r2 = r2_score(actual, predicted)
    return rmse, mae, r2
```

<!-- #region pycharm={"name": "#%% md\n"} -->
## Training
<!-- #endregion -->

Train a model using the training part of the dataset (train_x, train_y).

```python
alpha = 1.0
l1_ratio = 0.5
model = ElasticNet(alpha=alpha, l1_ratio=l1_ratio)

_ = model.fit(train_X, train_y)
pred_test_y = model.predict(test_X)
rmse, mse, r2 = compute_metrics(test_y, pred_test_y)
```

## Logging

<!-- #region pycharm={"name": "#%% md\n"} -->
### Create a new MLflow run
<!-- #endregion -->

Each of your individual entry to MLflow is referred to as a run. In order to log a next run you first need to call mlflow.start_run().

```python
_ = mlflow.start_run(run_name='custom_run_name')
```

### Set MLflow user name to your name

<!-- #region pycharm={"name": "#%% md\n"} -->
Each run has a user name tag specifying the user who performed the run. Use mlflow.set_tag('key', value) with key equal to mlflow.utils.mlflow_tags.MLFLOW_USER and value set appropriately (e.g. tkosek).
<!-- #endregion -->

```python pycharm={"name": "#%%\n"}
mlflow.set_tag(mlflow.utils.mlflow_tags.MLFLOW_USER, 'your_user_name')
```

<!-- #region pycharm={"name": "#%% md\n"} -->
### Log selected hyperparameters
<!-- #endregion -->

<!-- #region pycharm={"name": "#%% md\n"} -->
Log the hyperparameters of your model (mainly the ones that you were optimizing). This can be done by calling mlflow.log_param('param_name', value). The key and value are both strings (value will be but will be converted to string).
<!-- #endregion -->

```python pycharm={"name": "#%%\n"}
mlflow.log_param('alpha', alpha)
mlflow.log_param('l1_ratio', l1_ratio)
```

### Log selected metrics


Compute RMSE, MAE and R2 metrics on the test part (test_x, test_y) of the dataset (you can utilize compute_metrics helper function). Log the results by using mlflow.log_metric('metric_name', value). Use lowercased names (rmse, mae, r2).

```python pycharm={"name": "#%%\n"}
mlflow.log_metric('rmse', rmse)
mlflow.log_metric('mse', mse)
mlflow.log_metric('r2', r2)
```

### Log the model


You can save and load MLflow Models in multiple ways. First, MLflow includes integrations with several common libraries. For example, mlflow.sklearn contains save_model, log_model, and load_model functions for scikit-learn models. Second, you can use the mlflow.models.Model class to create and write models.

If you are using scikit-learn model, use mlflow.sklearn.log_model(model, 'model_name') to log the model.

```python pycharm={"name": "#%%\n"}
mlflow.sklearn.log_model(model, 'model')
```

### Log other artifacts


MLflow allows you to log a local file or directory as an artifact.

```python pycharm={"name": "#%%\n"}
mlflow.log_artifact('pyproject.toml')
mlflow.log_artifact('poetry.lock')
```

### End the run


Each MLflow run needs to be ended. Call mlflow.end_run() to end the active run.

```python pycharm={"name": "#%%\n"}
mlflow.end_run()
```

# MLflow UI


The Tracking UI lets you visualize, search and compare logged results. Visit http://127.0.0.1:5000 if you are running the MLflow server locally or http://[container_name].lely.dtml:5000 if you are running the MLflow server on the Lely cluster (e.g. http://jpallgpu.lely.dtml:5000).
