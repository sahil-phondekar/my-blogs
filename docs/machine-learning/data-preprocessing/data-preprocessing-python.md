---
sidebar_position: 2
---

# Data Preprocessing in Python

## Data Preprocessing tools

### Importing the libraries

We will need the following three libraries for most of the data pre-processing tasks

*   Numpy - Allows us to work with arrays
*   Matplotlib - Allows us to plot charts and graphs
*   Pandas - Allows us to import the dataset and create matrix of features and dependent vectors.

```python
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
```

### Importing the dataset

Step 1: Import the dataset   
Step 2: Create matrix of **feature** and **dependent variable vectors**

In any machine learing dataset we have two sets of data which are the features and the dependent vector variable.

The features are the columns with which you are going to predict the dependent variable.

The dependent variable is the column which you want the ML model to predict.

```python
dataset = pd.read_csv('Data.csv')
X = dataset.iloc[:, :-1].values
y = dataset.iloc[:, -1].values
```
```python
print(X)
```
```python
print(y)
```

### Taking care of missing data

One way to take care of missing data is to remove them. This is effective when you have a large dataset and only few of the data have missing values. Hence removing them will not impact the test data very much.

Second way is to replace the missing data with average of the whole column.

For this we will use one of best data science library **ScikitLearn**

It contains a lot of tools including lot of preprocessing tools.

```python
from sklearn.impute import SimpleImputer
imputer = SimpleImputer(missing_values=np.nan, strategy='mean')
imputer.fit(X[:, 1:3])
X[:, 1:3] = imputer.transform(X[:, 1:3])
```
```python
print(X)
```

### Encoding categorical data

The categorical data may most probably contain string data. So it is necessary to encode this data into numerical format for the machine learning model to interpret the result accurately.

One of the most popular method is **One-hot encoding**

If the column is categorized only into two values we can simply represent the values as binary format 1/0. This can be achieved by using **Label Encoding**

#### Encoding the independent variable

```python
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import OneHotEncoder
ct = ColumnTransformer(transformers=[('encoder', OneHotEncoder(), [0])], remainder='passthrough')
X = np.array(ct.fit_transform(X))
```
```python
print(X)
```

#### Encoding the dependent variable

```python
from sklearn.preprocessing import LabelEncoder
le = LabelEncoder()
y = le.fit_transform(y)
```
```python
print(y)
```

### Splitting the dataset into Training set and Test set

```python
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 1)
```
```python
print(X_train)
```
```python
print(X_test)
```
```python
print(y_train)
```
```python
print(y_test)
```

### Feature Scaling

Helps put all our features on the same scale.

```python
from sklearn.preprocessing import StandardScaler
sc = StandardScaler()
X_train[:, 3:] = sc.fit_transform(X_train[:, 3:])
X_test[:, 3:] = sc.transform(X_test[:, 3:])
```
```python
print(X_train)
```
```python
print(X_test)
```