<div style="text-align:center;">
    <img src="https://github.com/andytoh78/missing_values/assets/139482827/69bfcb61-4001-4716-b3be-f7c35b4b4075" alt="Image">
</div>

---

## **Missing Values Imputation**

Dealing with missing data effectively is crucial for the integrity of your analysis and model performance. Here are various strategies, ranging from simple to more complex techniques.

### **Listwise Deletion**
- Also known as complete case analysis, it involves removing any record with a missing value.
- This method is simple but can lead to significant data loss, especially if the missing values are spread across different variables.
- Should only be considered when the data is MCAR and the proportion of missing data is small.

```python
# Drop all rows with missing values
df.dropna(inplace=True)

# Drop rows where a specific column has missing values
df.dropna(subset=["column"], inplace=True)
``` 
### **Column Deletion**
- Also known as variable or feature deletion, it involves removing an entire column from the dataset if it contains missing values.
- Simmilar to listwise deletion, this method is simple but can lead to the loss of potentially valuable information.
- Should only be used for columns that exhibit significant missingness and/ or if the variable is deemed insignificant or not crucial for the analysis.
  
```python
# Drop all columns with missing values
df.dropna(axis=1, inplace=True)

# Drop specific columns with missing values
df.drop(["column1", "column2", ...], axis=1, inplace=True)
```
From the titanic dataset, we will use "Cabin", which has a high percentage of missing values (77%), as an example to demonstrate column deletion. In such cases, where a significant portion of the data is missing from a specific column, removing that column can be a practical decision in data preprocessing.

```python
# Import libraries
import numpy as np
import pandas as pd

# Load the titantic dataset
df=pd.read_csv("titanic_train.csv")

# Drop Cabin (77% missing values)
df2.drop(columns="Cabin", inplace=True)
```

### **Pairwise Deletion**
- This is a more conservative method than listwise deletion as it keeps the data intact and selectively excludes missing data only during specific statisical analyses such as correlations or regressions.
- Simmilar to listwise deletion, this method is simple but can lead to the loss of potentially valuable information.
- Should only be used when the missingness pattern varies across variables and is either MCAR or MAR.
  
```python
# Pairwise deletion in pandas for correlation
pairwise_corr = df.corr()
```
![image](https://github.com/andytoh78/missing_values/assets/139482827/a54f0ac3-1080-4669-997b-208ce6a903e3)

In this matrix, the correlation between "Age" and the rest of the numerical variables is calculated based on the rows where both "Age" and the respective variable have non-missing values. This method ensures that each correlation coefficient is derived from the maximum available data without introducing bias from rows with missing values in either of the two variables being compared. However, this can lead to biases if the pattern of missingness is not random (MCAR).

|**Method**|**Description**|**Categorical**|**Numerical**|**Pros**|**Cons**|**When to Use?**|
|:-|:-------|:-:|:-:|:--------------------|:-----------------------|:---------------------|
|**Listwise Deletion**|Remove records with missing values|Y|Y|Simple and quick|May result in significant data loss, especially if there's a lot of missingness|When missingness is completely at random and data loss is acceptable|
|**MEAN Imputation**|Replace with the mean of the observed values in the column|N|Y|Simple and quick|Does not consider relationships between the features. Not suitable for variables with outliers as this imputation method can introduce bias into the imputed values|When data is missing completely at random (MCAR) and the variable follows a normal distribution|
|**MEDIAN Imputation**|Replace with the median of the observed values in the column|Y<br>(Ordinal)|Y|Simple and quick|Does not consider relationships between the features. Though more robust than mean imputation, median imputation can still be influenced by extreme values in the variable, leading to biased imputed values|When data is missing completely at random (MCAR) and when the distribution of the variable with missing column is skewed or contains outliers|
|**MODE Imputation**|Replace with the most frequent value in the column|Y|N|Simple and quick|Does not consider relationships between the features and may not always provide representative results|Suitable for categorical data with frequent mode values|
|**Regression Imputation**|Predicts missing values using regression equations based on other available variables|Y|Y|Accounts for relationships between variables. Allows for detailed modeling of missing data|Requires the assumption that a linear relationship exists between variables. Does not account for uncertainty associated with imputed values. Can be sensitive to model specification|When data is NOT missing completely at random (MCAR) and can be explained by other variables.|
|**MICE Imputation**|Multiple Imputation by Chained Equations (MICE) method|Y|Y|Accounts for relationships between features.<br>Provides multiple imputations for uncertainty estimation.<br>Versatile. Suitable for various types of missing data|Can be computationally intensive|When data relationships and when the missing data is missing at random (MAR) - otherwise it may introduce bias into the imputed values|
|**MICE with LightGBM (miceforest)**|Multiple Imputation by Chained Equations using LightGBM as a model|Y|Y|Leverages power of gradient boosting, can handle mixed data types|Computationally intensive and requires careful tuning|When a robust imputation method is desired and computational resources are available|
|**MissForest Imputation**|Utilizes Random Forests to estimate missing values|Y|Y|Efficient for large datasets and can handle complex data structures and variable types|May not provide detailed uncertainty estimates. Sensitivity to hyperparameters. Less interpretable than traditional methods|When dealing with large datasets, complex data structures, and when speed and efficiency are critical|
|**KNN Imputation**|Use the k-nearest neighbors to impute missing values|Y|Y|Can capture complex patterns in the data|Computationally expensive, especially for large datasets|When other simpler methods don't perform well and enough computational power is available|
|**Interpolation**| Fill missing values using various interpolation techniques such as LOCF (Last Observation Carried Forward) and NOCB (Next Observation Carried Backward)|N|Y|-| Effective for time series data with trends| Assumes a specific pattern or trend, may not be valid for all datasets. Can introduce bias, especially in trending or seasonal data|Time series data with consistent trends|
|**Random Imputation**|Replace missing values with random values from the dataset|Y|Y|Simple and quick|May introduce randomness and not leverage patterns in the data|As a temporary solution or baseline|
|**Constant Imputation**|Replace missing values with a constant|Y|Y|Can be effective for categorical variables to highlight missingness|Does not use any information from the data. Can distort distributions|To explicitly highlight missingness|
|**End of Distribution Imputation**|Replace with values at the extreme end|N|Y|Can be effective if missingness is associated with 'extremeness'|May introduce outliers. Distorts the original distribution|When missingness might be related to extreme behavior|
