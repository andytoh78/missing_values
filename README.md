<div style="text-align:center;">
    <img src="https://github.com/andytoh78/missing_values/assets/139482827/69bfcb61-4001-4716-b3be-f7c35b4b4075" alt="Image">
</div>

---
Missing Values refers to the **absence of data or information for specific variables or observations within a dataset**. Their significance lies in their potential to **compromise data quality and integrity**. Incomplete data can lead to undesirable consequences, including the potential for **inaccurate and biased analyses**, which, in turn, can **undermine the reliability of data-driven decision-making**. In certain scenarios, they may even lead to **ethical issues such as discrimination** or **non-compliance, potentially resulting in legal repercussions**. Missing data **often carries valuable information**, and it can result in a **loss of insights and patterns** if not handled properly. Moreover, the **accuracy and performance of predictive machine learning models can also be affected** by the presence of missing values.

&nbsp;

> [!TIP]
> **Missingness** refers to the condition where data values are absent or not recorded in a dataset. **Not all missing values come as blanks or null values**. In some datasets, the missing values may also be represented by other placeholders such as as "0", "NA", "N/A", "na", "?", "undefined", "Not given" and so on. For example, a value of "0" in the 'Fare' column could indicate a missing or unrecorded fare. It is, therefore, important to inspect the unique values for each column in the dataset and use domain knowledge to identify suspicious values that might be acting as placeholders for missing data.


&nbsp;

## **Understanding Types of Missing Values**
---

Missing data can be classified into several types based on the underlying patterns and causes of the missingness. Understanding the types of missing data is crucial for effective data analysis and the selection of appropriate imputation strategies that minimize bias and inaccuracies.

|**TYPE**|**PATTERN**|**IDENTIFICATION**|**EXAMPLES**|**PROBLEMATIC LEVEL**|**POSSIBLE RESOLUTION**|
|:-|:-|:-|:-|:-|:-|
|**MCAR**<br>**(Missing Completely At Random)**|Missingness is completely random and **unrelated to any observed or unobserved data** i.e., the probability of missingness is the same for all observations and the data is missing purely by chance due to human or technical errors|Typically tested using statistical methods. A common method is to compare missing and non-missing data groups to determine if they are statistically different|Missing blood pressure readings from a random sample of patients in a clinic|**Low**<br>Does not introduce systematc bias|Simple imputation methods like mean/mode/median imputation or random imputation|
|**MAR**<br>**(Missing At Random)**|Missing data is not completely random and the missingness of data **has some dependencies on the observed data**| Typically assumed rather than directly observed. Statistical tests can be conducted to see if there are significant differences between missing and non-missing data groups|In a medical study, the probability of missing data on a health survey may depend on age and gender, which are observed|**Low**<br>Does not introduce systematc bias|Advanced imputation methods that consider relationships with observed data e.g., KNN imputation, regression imputation, MICE imputation, MissForest imputation|
|**MNAR/NMAR (Missing Not At Random)**|Missingness **depends not only on the observed but also the unobserved data** or the values of the missing data|Challenging to identify and often requires a deep understanding of the data and the specific domain. Statistical tests alone may not be sufficient to detect MNAR|In a survey on income, high-income earners might be less likely to disclose their income|**High**<br>Significant risk of bias|Sophisticated modeling approaches and sensitivity analysis - complex process that goes beyond standard imputation methods|

&nbsp;

![image](https://github.com/andytoh78/missing_values/assets/139482827/7be7895f-4fab-442d-a9b9-00c14027f12b)

&nbsp;

## **Identifying Missing Data with `missingno`**
---

The missingno library is a useful tool for visualizing and understanding missing values in a dataset. It is especially useful in the exploratory data analysis phase for uncovering patterns of missingness, which can help drive the strategy for handling the missing values. 

**Bar Plot**
- Displays the **percentage of missing values for each column** in the dataset
- Helps **identify columns with high proportion** of missing values

**Matrix Plot**
- Visualize the **locations and patterns** of missingness, like whether the missing values are clustered or spred randomly across the dataset
- **White lines indicate missing values**

**Heatmap**
- Compute and visualize the **correlation of missingness (range between -1 and 1) between pairs of columns with missing values**
- It **does not include columns without missing values**. Columns without missing values are typically not the focus of this visualization because their missingness is not relevant in this context
- The primary purpose is to **understand whether the missingness in one column is related to the missingness in other columns**
- This insight will be useful in providing guidance on the imputation strategies

|**Correlation**|**Description**|
|:--|:--|
|**1**|Suggest that when one variable is missing, the other is likely to be missing as well|
|**0**|Suggest that the missingness of one variable does not affect the likelihood of missingness in another|
|**-1**|Suggest that when one variable is missing, the other is likely to be present|

**Dendrogram**
- Tree-like diagram that shows the clustering of variables. It uses hierarchical clustering algorithms to group variables based on the similarity of their missing value patterns
- It is useful in identifying whether the pattern of missingness is random or systematic

&nbsp;

To illustrate, we will use the missingno library to explore and analyse the missingness of data in the [Titanic dataset](https://www.kaggle.com/competitions/titanic/data).

```python
# Import libraries
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

# Load the titanic dataset
df=pd.read_csv("titanic_train.csv")
print(f"The dataset has {df.shape[0]} rows and {df.shape[1]} columns.")
```
The dataset has 891 rows and 12 columns.

```python
# Check for missing values
missing_values = df.isnull().sum()
missing_values_df = pd.DataFrame(missing_values, columns=["Count"])
missing_values_df["% Missing"] = ((missing_values_df["Count"]/len(df))*100).round(2)
missing_values_df[missing_values_df["Count"]>0]
```
> |              | Count     |  % Missing    |
> |:-------      |:----------| :------------ |
> | **Age**	     | 177       | 19.87         |
> | **Cabin**    | 687       | 77.10         |
> | **Embarked** | 2         | 0.22          |

```python
# Install missingno
pip install missingno
```
```python
# Import missingno
import missingno as msno
```
