
# Bank churn prediction portfolio

![alt text](https://github.com/hyj-main/portfolio_bank_churn/blob/master/fig/bank_churn.png)

## 1. Purpose
'Customer churn' cab be defined as customers quitting services for various reasons; even including involuntary exit. In bank industry, customer churn may happen when they are not satisfied with bank service, or when they are not able to maintain their bank accounts, which may be associated with factors such as customer's gender, age, credit score, balances and whether being an active member, etc. For banks, it is a key to predict customers who are likely to churn to retain certain level of customer numbers because generally, gaining a new customer is more difficult than preventing them churn out. The main purpose of this project is to identify features of customers who has higher possiblity to churn and obtain business insights to increase retention rate of existing customers.


## 2. Data description

The data was downloaded from a Kaggle website below: 

https://www.kaggle.com/datasets/gauravtopre/bank-customer-churn-dataset

The dataset consists of following features:

* customer_id: Bank account number of customers
* credit_score: credit score of customers
* country: Name of countries where customers live in
* gender: Either Male of Female
* age: age of customers
* tenure: The number of years that a customer has been keeping a bank account
* balance: balance remaining in an account
* products_number: The number of products a customer purchased from the bank
* credit_card: Whether a customer possesses a credit card
* active member: Whether a customer is an active member


## 3. EDA
This is a summary of EDA. To see the detailed version, please click the following link.

[EDA Jupyter Notebook (<= Click me)](https://nbviewer.org/github/hyj-main/portfolio_bank_churn/blob/master/bankchurn_EDA_20221231.html)


* a. check missing values for all columns
    - No missing values were found throughout all columns

* b. Distribution of target variable showed highly unbalnced pattern; most customers did not churn out. This will be considered in the model prediction part. 

![alt text](https://github.com/hyj-main/portfolio_bank_churn/blob/master/fig/churn_dist.png)

* c.Correlation between features and target (churn behavior)
    - 100% of customers with credit score below 400 churned out.
![alt text](https://github.com/hyj-main/portfolio_bank_churn/blob/master/fig/fig1.png)

    - Across all age groups (bin size = 10), females tended to churn more than males.
![alt text](https://github.com/hyj-main/portfolio_bank_churn/blob/master/fig/agegroup_gender.png)

    - Mid-aged customers (40s-50s) tended to churn more than the other groups.
![alt text](https://github.com/hyj-main/portfolio_bank_churn/blob/master/fig/midvsnonmid.png)

    - Customers with higher balance tended to churn more.
![alt text](https://github.com/hyj-main/portfolio_bank_churn/blob/master/fig/balance.png)

    - Customers who had products more than 3 showed high churn rate. Specifically. when the product nubmer was 4, 100% of customers churned out.

![alt text](https://github.com/hyj-main/portfolio_bank_churn/blob/master/fig/pnum.png)

    - Non-active customers tended to churn more than active customers.
![alt text](https://github.com/hyj-main/portfolio_bank_churn/blob/master/fig/act.png)


## 4. Feature engineering
* a. Cateogrical features
    - One-Hot-Encoding for gender
* b. Numerical features
    - Added a variable that shows whether mid-aged(1) or not(0).
    - Added age groups with bin size of 10 
    - Standardization of all numerical features


## 5. Model development with OPTUNA
This is a summary of model development. To see the detailed version, please click the following link.

[Model development Jupyter Notebook (<= Click me)](https://github.com/hyj-main/portfolio_bank_churn/blob/master/bankchurn_model_prediction.ipynb)


* XGBoost classifer model was chosen becuase as an ensemble tree models combined with weaker models, it shows better classification accuracy. 
* To handle imbalanced distribution of churn, did not use mere 'accuracy' as an evaluation metrics. Instead, 'AUC' and 'AUCPR' were chosen.
* Best set of hyperparameters were chosen by optuna. 
* To prevent a potential over-fitting problem, early stopping option was used.
* XGBoost classifer with feature engineering showed an AUC score of 0.85 and AUPRC of 0.68, which showed a robust classification performance.

![alt text](https://github.com/hyj-main/portfolio_bank_churn/blob/master/fig/roc.png)

## 6. Conclusion

The following business insights were obtained from EDA and XGBoost Classifer prediction.

- Credit score
    - From the beginning, not accepting customers with low credit score below 400.
    - Once the bank accepts a customer, bank could provide alerts that can damage credit score.

- Gender and Age
    - Generally, Mid-aged groups would earn highest income, compared to younger or older groups. Hence it would be crucial to motivate mid-aged groups not to quit bank service.
    - Active promotions events for mid-aged women might help lowering thier churn rate.

- Balance 
    - Maintaining customers who have higher balance would be more beneficial to bank management, but it turned out that they are more likey to churn. Therefore, special attention would be necessary to make them stay.

- Products number
    - Keep the number of products simple as 1 or 2.

- Active member
    - Bank may create activeness index and monitor periodically to have more customers active. To do so, defining 'being an active member' is the very first step. It can be defined by the amount of balance, the number of transaction during a certain period, number of online access per day, etc.Depending on the definition, the bank may hold an event to reward customers who increase activeness. 
