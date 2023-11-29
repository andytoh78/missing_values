# **Missing Values**
---
${\textbf{\color{yellow}Missing Values}}$ refers to the **absence of data or information for specific variables or observations within a dataset**. Their significance lies in their potential to **compromise data quality and integrity**. Incomplete data can lead to undesirable consequences, including the potential for **inaccurate and biased analyses**, which, in turn, can **undermine the reliability of data-driven decision-making**. In certain scenarios, they may even lead to **ethical issues such as discrimination** or **non-compliance, potentially resulting in legal repercussions**. Missing data **often carries valuable information**, and it can result in a **loss of insights and patterns** if not handled properly. Moreover, the **accuracy and performance of predictive machine learning models can also be affected** by the presence of missing values.


> [!TIP]
> Not all missing values come as blanks or null values. In some datasets, the missing values may also be represented by other placeholders such as as "0", "NA", "N/A", "na", "?", "undefined", "Not given" and so on. For example, a value of "0" in the 'Fare' column could indicate a missing or unrecorded fare. It is, therefore, important to inspect the unique values for each column in the dataset and use domain knowledge to identify suspicious values that might be acting as placeholders for missing data.


## <br>**Types of Missing Data**
---

Missing data can be classified into several types based on the underlying patterns and causes of the missingness. Understanding the types of missing data is crucial for effective data analysis and the selection of appropriate imputation strategies that minimize bias and inaccuracies. 

|**Type**|**Pattern**|**Identification**|**Examples**|**Problematic Level**|**Possible Resolution**|
|:-|:-|:-|:-|:-|:-|
|**MCAR**<br>**(Missing Completely At Random)**|Missingness is completely random and **unrelated to any observed or unobserved data** i.e. missing purely by chance due to human or technical errors|Typically tested using statistical methods. A common method is to compare missing and non-missing data groups to determine if they are statistically different|Missing blood pressure readings from a random sample of patients in a clinic|**Low**<br>Does not introduce systematc bias|Addressed through imputation methods|
|**MAR**<br>**(Missing At Random)**|Missing data is not completely random and the missingness of data **has some dependencies on observed data**|Typically assumed rather than directly observed. Statistical tests can be conducted to see if there are significant differences between missing and non-missing data groups|In a medical study, the probability of missing data on a health survey may depend on age and gender, which are observed|**Low**<br>Does not introduce systematc bias|Addressed through imputation methods that consider relationships with observed data e.g. regression imputation, mice imputation, missforest imputation|
|**MNAR**<br>**(Missing Not At Random)**|Missingness **depends not only on observed but also unobserved data** or the values of the missing data|Challenging to identify and often requires a deep understanding of the data and the specific domain. Statistical tests alone may not be sufficient to detect MNAR|In a survey on income, high-income earners might be less likely to disclose their income|**High**<br>Significant risk of bias|Sophisticated modeling approaches and sensitivity analysis - complex process that goes beyond standard imputation methods|
|**NMAR**<br>**(Not Missing At Random)**|A subset of MNAR where missingness is **related to unobserved data** or the values of the missing data in a specific way|Like MNAR, NMAR is challenging to identify and may require domain knowledge and sensitivity analysis|In a study on spending behavior, participants with high debt may be more likely to withhold financial data|**High**<br>Significant risk of bias|Sophisticated modeling approaches and sensitivity analysis - complex process that goes beyond standard imputation methods|

 
```python
pip install missingno
```
