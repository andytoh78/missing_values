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
- Displays the **percentage of missing values for each column** in the dataset.
- Helps **identify columns with high proportion** of missing values.

**Matrix Plot**
- Visualize the **locations and patterns** of missingness, like whether the missing values are clustered or spred randomly across the dataset.
- **White lines indicate missing values**.

**Heatmap**
- Compute and visualize the **correlation of missingness (range between -1 and 1) between pairs of columns with missing values**.
- It **does not include columns without missing values**. Columns without missing values are typically not the focus of this visualization because their missingness is not relevant in this context.
- The primary purpose is to **understand whether the missingness in one column is related to the missingness in other columns**.
- This insight will be useful in providing guidance on the imputation strategies.

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
display(df.head())
print(f"The dataset has {df.shape[0]} rows and {df.shape[1]} columns.")
```
![image](https://github.com/andytoh78/missing_values/assets/139482827/8fe28508-836b-4adc-812d-d7a7be02aa1d)

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
### **${\color{black}\textsf{Bar Plot}}$**

```python
msno.bar(df, figsize=(16, 3), fontsize=10)
plt.show()
```

![image](https://github.com/andytoh78/missing_values/assets/139482827/e73e3301-431b-4cfa-ba8b-239ef14f6a14)

The length of the each bar corresponds to the number of non-missing values for the column and the white spaces above indicates missing values. From the visual representation, 
- Cabin has the highest number of missing values, followed by Age.
- Embarked has 2 missing values.
- All other columns seem to have no missing values.

### **${\color{black}\textsf{Matrix Plot}}$**

```python
msno.matrix(df, figsize=(16, 4), fontsize=10)
plt.show()
```

![image](https://github.com/andytoh78/missing_values/assets/139482827/fa444ec6-a00b-493c-9e39-39c24167cf87)

From the matrix plot, the missing values for **Age and Cabin appear to be randomly distributed**, which suggests that the **missingness could be either Missing At Random (MAR) or Missing Completely At Random (MCAR)**. **Embarked has only 2 missing values and they can be assumed to be Missing Completely At Random (MCAR)**.

To get a better understanding of the patterns or relationships between the missing values across different columns, we will sort the dataset by one column at a time and then visualize the missing values using msno.matrix().

```python
# Create a function to sort dataset by column and plot msno.matrix()
def visualize_missingness_by_column(df):
    for col in df.columns:
        # Sort dataframe by the column
        df_sorted=df.sort_values(by=col)
        
        # Plot the missing values using msno.matrix()
        msno.matrix(df_sorted,  figsize=(16, 4), fontsize=10)
        plt.title(f"Missingness Sorted by {col}", fontsize=14, fontweight=700)
```
![image](https://github.com/andytoh78/missing_values/assets/139482827/bc9c890b-db9e-47da-8cf5-5857588484b9)

- Each row represents a passenger.
- Each column represents a variable in the dataset.
- White lines indicate missing values.
- Black regions indicate non-missing values.
- The sparkline at the right summarizes the general shape of the data completeness and points out the rows with the maximum and minimum missing data.

From the matrix plots, 
- The **missing values for "Embarked" are likely to be Missing Completely At Random (MCAR)**. This assumption is based on the small number of missing values and the absence of any clear patterns for their missingness.</p>
- The **missing values for "Age" is also likely to be Missing Completely At Random (MCAR)**  as the missing values are randomly distributed in all the matrix plots.</p>
- The **missingness for "Cabin" seems to have an association with the "Pclass" (passenger class) and "Fare"**. Passengers in higher classes, such as first class, who likely paid higher fares for their tickets, are more likely to have cabin information available. Therefore, the **missing values for "Cabin" are likely to be Missing at Random (MAR)** and the missingness can be predicted from the observed data in  "Pclass" and "Fare".

Statistical methods can be used to test for association between the missingness in one variable and the values in another variable. 
- Categorical vs. Categorical : Chi-squared test
- Categorical vs. Numerical : T-test for two categories or ANOVA for more than two categories
- Numerical vs. Numerical : Pearson correlation coefficient

### **${\color{black}\textsf{Chi-Squared Test}}$**

Let's delve deeper to see if there is indeed a significant association between Cabin and Pclass.

```python
from scipy.stats import chi2_contingency

# Convert the missingness of 'Cabin' into a binary categorical variable (1 for missing, 0 for not missing)
df['Cabin_Missing'] = df['Cabin'].isnull().astype(int)

# Create a contingency table that cross-tabulates this binary variable with 'Pclass'
contingency_table = pd.crosstab(df['Cabin_Missing'], df['Pclass'])
print("Contingency Table")
display(contingency_table)

# Perform the chi-squared test
chi2_stat, p_val, dof, ex = chi2_contingency(contingency_table)

# Interpret the p-value
significance_level = 0.05
if p_val <= significance_level:
    result = "The missingness of Cabin is associated with Pclass."
else:
    result = "No significant association found between the missingness of Cabin and Pclass."

print(f"Chi-squared Statistic: {chi2_stat}, P-value: {p_val}\nResult: {result}")
```
Contingency Table
| Pclass<br>Cabin_missing | 1 | 2 | 3 |
|:-----|:---:|:---:|:---:|
| 0 | 176 | 16 | 12 |
| 1 | 40 | 168 | 479 |

Chi-squared Statistic: 557.3102629401849, P-value: 9.585484042103076e-122<br>
Result: **The missingness of Cabin is associated with Pclass**.

### **${\color{black}\textsf{T-test}}$**

We will now take a look at Cabin and Fare. 

```python
# Use T-test to test for association between the missingness of Cabin and Fare
from scipy.stats import ttest_ind

# Split the Fare data into two groups (with missing vs non-missing Cabin)
group1 = df[df['Cabin_Missing'] == 0]['Fare']
group2 = df[df['Cabin_Missing'] == 1]['Fare']

# Perform the T-test
t_stat, p = ttest_ind(group1, group2, equal_var=False)  # Assuming unequal variances with Welch's t-test

print(f"T-statistic = {t_stat}")
print(f"P-value = {p}")

if p < 0.05:
    print("\nThere is a significant difference in 'Fare' between passengers with and without cabin information.")
else:
    print("\nThere is no significant difference in 'Fare' between passengers with and without cabin information.")
```
T-statistic = -10.707225705301752
P-value = 7.974017535465969e-22

There is a significant difference in 'Fare' between passengers with and without cabin information.

```python
import seaborn as sns
# Set up the figure and axes
fig, ax = plt.subplots(nrows=2, ncols=1, figsize=(10,7), gridspec_kw={'height_ratios': [1, 2]}, sharex=True)

# Boxplot on top
sns.boxplot(data=[group1, group2], orient='h', width=0.7, notch=True, showmeans=True, meanline=True,
            boxprops={"linewidth": 1.5, "edgecolor": "black"},
            meanprops={"linestyle": ':', "linewidth": 1.5, "color": "navy"},
            flierprops={"marker": "o", "markerfacecolor": "darkblue", "markersize" : 6}, 
            ax=ax[0], palette=["limegreen", "coral"])
ax[0].set_yticklabels(['Fare_with_Cabin', 'Fare_without_Cabin'])
ax[0].set_xlabel('') 
ax[0].set_title("Distribution of Fares vs Cabin\n", fontsize=14, fontweight=600)

# Histogram below
sns.histplot(group1, ax=ax[1], color="limegreen", stat="density", linewidth=0.1, kde=True, label="Fare_with_Cabin")
sns.histplot(group2, ax=ax[1], color="coral", stat="density", linewidth=0.1, kde=True, label="Fare_without_Cabin")
sns.kdeplot(group1, ax=ax[1], color="forestgreen", linewidth=3)
sns.kdeplot(group2, ax=ax[1], color="red", linewidth=3)
ax[1].set_ylabel("Density\n", fontsize=12, fontweight=600)
ax[1].set_ylim(0, 0.05)
ax[1].legend()

# Adjust layout
plt.tight_layout()
plt.show()
```
![image](https://github.com/andytoh78/missing_values/assets/139482827/ea3f79dd-d2f1-460c-adf6-7812d083dbb4)

- The boxplot clearly shows different means and medians for the two groups ("Fare_with_Cabin" and "Fare_without_Cabin")
- The distribution of fares for passengers with cabin information is generally higher than that of passengers without cabin information. The presence of many outliers in the "Fare_without_Cabin" group suggests that while most passengers without cabin details paid lower fares, there were still a few who paid higher fares.
- The density plots below further confirm these observations, where the peak for "Fare_with_Cabin" is shifted to the right (indicating higher fares) compared to the peak for "Fare_without_Cabin".
- Overall, these plots have **demonstrated significant difference in the 'Fare' between passengers with and without cabin information**. We will further validate this using T-test.

### **${\color{black}\textsf{Heatmap}}$**

```python
msno.heatmap(df, figsize=(6, 5), cmap='magma', fontsize=10)
plt.show()
```
<img src="https://github.com/andytoh78/missing_values/assets/139482827/445a803b-afc8-45c7-9200-e40b84504e96" width="500" height="400">

The heatmap suggests that the **missingness for each of the columns with missing values ["Cabin", "Age", "Embarked"] is not strongly associated with the missingness of the other columns**.

### **${\color{black}\textsf{Dendrogram}}$**

```python
msno.dendrogram(df, figsize=(8, 4), fontsize=10)
plt.show()
```

![image](https://github.com/andytoh78/missing_values/assets/139482827/dad8e06f-16be-4afa-aaff-b8fe861c518d)

- The **height of each branch corresponds to the number of missing values** in the respective columns i.e. **"Cabin" has the highest count of missing values, followed by "Age" and "Embarked"**.
- **Columns that are connected by a branch are considered to be in a cluster** i.e. they have similar missingness patterns.
- The **distance between branches represents the dissimilarity between columns in terms of missingness**. The shorter the distance, the more similar their missingness patterns are. **"Pclass", "Survived", "Sex", "SibSp", "Parch", "Ticket", "Fare", and "Embarked" are connected and they form a cluster, indicating they have similar patterns of missing data. "Age" and "Cabin" are isolated, meaning their missingness patterns are not particularly similar to any of the other columns**.

Now that we have an understanding of the various types of missing data, the following section will elaborate on the strategies for managing missingness, encompassing a spectrum of imputation methods.
