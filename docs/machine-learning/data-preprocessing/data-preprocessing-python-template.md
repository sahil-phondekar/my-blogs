---
sidebar_position: 3
---

# Data Preprocessing Template

## Importing the libraries

```python
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
```

## Importing the dataset

```python
dataset = pd.read_csv('Data.csv')
X = dataset.iloc[:, :-1].values
y = dataset.iloc[:, -1].values
```

## Splitting the dataset into Training set and Test set

```python
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
```