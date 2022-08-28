# Credit Line Increase Model Card

### Basic Information

* **Person or organization developing model**: Runtong Jiang, `runtong1@gwu.edu`, Abdulrahman Alsadun , `abdulrahman.alsadun@gwu.edu` , Emmanuel Asong, `emmanuelasong@gwu.edu`, Ying Hao, `yinghao25@gwu.edu`
* **Model date**: August, 2022
* **Model version**: 1.0
* **License**: MIT
* **Model implementation code**: [DNSC_6301_Group_29.ipynb](DNSC_6301_Group_29.ipynb)

### Intended Use
* **Primary intended uses**: This model is an *example* probability of default classifier, with an *example* use case for determining eligibility for a credit line increase.
* **Primary intended users**: Students in GWU DNSC 6301 bootcamp.
* **Out-of-scope use cases**: Any use beyond an educational example is out-of-scope.

### Training Data

* **Source of training data**: GWU Blackboard, email `jphall@gwu.edu` for more information
* **How training data was divided into training and validation data**: 50% training, 25% validation, 25% test
* **Number of rows in training and validation data**:
  * Training rows: 15,000
  * Validation rows: 7,500

* **Data dictionary**: 

| Name | Modeling Role | Measurement Level| Description|
| ---- | ------------- | ---------------- | ---------- |
|**ID**| ID | int | unique row indentifier |
| **LIMIT_BAL** | input | float | amount of previously awarded credit |
| **SEX** | demographic information | int | 1 = male; 2 = female
| **RACE** | demographic information | int | 1 = hispanic; 2 = black; 3 = white; 4 = asian |
| **EDUCATION** | demographic information | int | 1 = graduate school; 2 = university; 3 = high school; 4 = others |
| **MARRIAGE** | demographic information | int | 1 = married; 2 = single; 3 = others |
| **AGE** | demographic information | int | age in years |
| **PAY_0, PAY_2 - PAY_6** | inputs | int | history of past payment; PAY_0 = the repayment status in September, 2005; PAY_2 = the repayment status in August, 2005; ...; PAY_6 = the repayment status in April, 2005. The measurement scale for the repayment status is: -1 = pay duly; 1 = payment delay for one month; 2 = payment delay for two months; ...; 8 = payment delay for eight months; 9 = payment delay for nine months and above |
| **BILL_AMT1 - BILL_AMT6** | inputs | float | amount of bill statement; BILL_AMNT1 = amount of bill statement in September, 2005; BILL_AMT2 = amount of bill statement in August, 2005; ...; BILL_AMT6 = amount of bill statement in April, 2005 |
| **PAY_AMT1 - PAY_AMT6** | inputs | float | amount of previous payment; PAY_AMT1 = amount paid in September, 2005; PAY_AMT2 = amount paid in August, 2005; ...; PAY_AMT6 = amount paid in April, 2005 |
| **DELINQ_NEXT**| target | int | whether a customer's next payment is delinquent (late), 1 = late; 0 = on-time |


### Test Data
* **Source of test data**: GWU Blackboard, email `jphall@gwu.edu` for more information
* **Number of rows in test data**: 7,500
* **State any differences in columns between training and test data**: None

### Model details
* **Columns used as inputs in the final model**: 'LIMIT_BAL',
       'PAY_0', 'PAY_2', 'PAY_3', 'PAY_4', 'PAY_5', 'PAY_6', 'BILL_AMT1',
       'BILL_AMT2', 'BILL_AMT3', 'BILL_AMT4', 'BILL_AMT5', 'BILL_AMT6',
       'PAY_AMT1', 'PAY_AMT2', 'PAY_AMT3', 'PAY_AMT4', 'PAY_AMT5', 'PAY_AMT6'
* **Column(s) used as target(s) in the final model**: 'DELINQ_NEXT'
* **Type of model**: Decision Tree 
* **Software used to implement the model**: Python, scikit-learn
* **Version of the modeling software**: 
  * Python version: 3.7.13
  * sklearn version: 1.0.2
* **Hyperparameters or other settings of your model**: 
```
{'ccp_alpha': 0.0,
 'class_weight': None,
 'criterion': 'gini',
 'max_depth': 6,
 'max_features': None,
 'max_leaf_nodes': None,
 'min_impurity_decrease': 0.0,
 'min_samples_leaf': 1,
 'min_samples_split': 2,
 'min_weight_fraction_leaf': 0.0,
 'random_state': 12345,
 'splitter': 'best'}
```
### Quantitative Analysis

* **Correlation Heatmap**: 
From the this heatmap we can see the correlation of the variables for the data
We can notice very clearly a high correlation between race and whether a customer's next payment is delinquent (late), which in fact raises serious discrimination issues.

![Correlation Heatmap](https://user-images.githubusercontent.com/111534710/186795237-aab0ce0e-a945-49ac-9369-03883de1a08d.png)

* **Tree depth vs. training and validation AUC**:
We can see that the division between the training AUC and the validation AUC start between 4 to 6 tree depth.

![Training   Validation AUC](https://user-images.githubusercontent.com/111534710/186795541-95c9ce93-e971-40a1-8b70-de5db3b5d601.png)

* **variable importances**:
This is to show the importance of each variable, and we can see clearly the heavy impact of the Pay_0 (most recant payment states)
although it is high impact that leads to high dependency on this variable, we think it make sense because if we want to know the person next payment states we look at their last one.

![Varibale Importance](https://user-images.githubusercontent.com/111534710/187083601-c2ebf09b-2c4b-406f-86e5-c9656cd65138.png)

* **Tree depth vs. training and validation AUC with Hispanic-to-White AIR**:
This iteration plots shows the relationship between training AUC, validation AUC and our least fortunate compared to most fortunate group AIR (Hispanic-to-White AIR).
And well helps us to make sure to maximize the fairness when choosing the depth

![Training   Validation AUC + H to W AIR](https://user-images.githubusercontent.com/111534710/187084313-128486f5-4d90-4572-a4c4-dc7cc03ef90c.png)

* **Area Under the Curve**:

Accepted range between 0.6 to 0.8

| Data | AUC |
| ---------------------- | ----------------------- |
|**Training**| 0.7837 |
| **Validation** | 0.7496 |
| **Test** | 0.7438 |

* **Adverse Impact Ratio**:

| Group | AIR |
| ---------------------- | ----------------------- |
|**hispanic-to-white**| 0.83 |
| **black-to-white** | 0.85 |
| **asian-to-white** | 1.00 
| **female-to-male** | 1.02 |


#### Ethical considerations


* **potential negative impacts for using the model**:

Our model’s AUC is 0.7438, which is higher than average, but there are still 0.2562 percent of the data that are not accurate. For our model, the database is not huge, so the accuracy problem is not obvious, but in the real world, 0.2 percentage uncertain prediction can cause lots of money loss. So, the model still needs to be analyzed and perfected.
There is a risk in using this model. when the model is used to take any decisions regarding giving credits and other financial services, certain group of people will not have equal outcomes. And that is because there is a bias against the Hispanic and Black people. Thus, it will make people become unsatisfied and avoid patronizing the service of the financial firms that use the model. It can also make government officials bring charges against the firm for bias and discrimination.

* **potential uncertainties relating to the impacts of using the model**:

Security and privacy issues may give rise to strong public interest against the firm. For the decision tree, if there is a change, even a little change in classification, the whole model will be changed a lot. So, the level of safety of the model is low. If some attackers tried to change the model to get the loan, the whole model would be destroyed. And the vicious attacks are hard to be detected.

* **Describe any unexpected or results**:

There are no missing values in the dataset. However, in the real-world missing values will exist which requires more cleaning. Also, the variable Pay_0 (most recant payment states) has a high importance for the model.

