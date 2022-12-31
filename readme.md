

# Bank churn prediction portfolio

## 1. Purpose

Predicting salary is useful in two aspects.
First, when people search for their jobs on job boards such as glassdoor, there are no explicit descriptions on salary, making it hard to make a choice to apply. Even if the exact amount of salary is shown, there is no way to assess whether that amount is legitimate without developing a prediction model.
Second, from HR's perspective, it is also hard for them to decide the amount of salary based on location of the office, job type, years of experience and so on. Therefore, it is important to predict salary for job seekers and HRs.


## 2. Data description
* jobId: primary key for identifying distinct jobs
* companyId: unique key for identifying distinct companies
* jobType: hierarchy levels in jobs ranging from Janitor and  junior to C-level titles
* degree: level of education such as high school, bachelors, masters etc.
* major: area of specific study subject such as math and biology.
* industry: specific part in which a company aims to make profit, such as oil, web and health 
* yearsExperience: years of job experience shown as an integer starting from 0.
* milesFromMetropolis: distance between workplace and metropolice area shown as an integer starting from 0



## 3. EDA
This is a summary of EDA. To see the detailed version, please click the following link.

[EDA Jupyter Notebook](https://nbviewer.org/github/hyj-main/portfolio_bank_churn/blob/master/bankchurn_EDA_20221230.html)


* a. check missing values for all columns
    - No missing values were found throughout all columns
* b. check duplicate values
    - No duplicate data were detected from jobId


* c. Distribution
    - Distributions of train and test dataset were almost identical. Therefore, overfitting is less likely to occur.
    - Distribution of salary was a little skewed to the right. When the salary was square-rooted, it got closer to normal distribution, indicating square-rooted version should be more accurate in prediction.
    - Invalid observations (Criteria for “invalidity”: e.g. negative observation for positive values, characters & numeric values mixed up.)
        -  5 data points with salaries that were the same or lower than 0, which were excluded from data.

        - Low job positions with extremelely high salaries
            - There were 20 Juniors with high salary
            - Among them, most degrees were in graduate school level (either master or doctoral) which was abnormal because in most cases, graduate school level degrees would result in higher job positions than junior. Therefore, 19 observations were removed from training dataset.


![alt text](https://github.com/hyj-main/portfolio_salary_pred/blob/master/image/1_1.png)

![alt text](https://github.com/hyj-main/portfolio_salary_pred/blob/master/image/1_2.png)

![alt text](https://github.com/hyj-main/portfolio_salary_pred/blob/master/image/1_3.png)


* d. Correlation between features and target (salary)
    - JobType was the most prominant feature that showed strongest correlation with salary than any other features. As we can use our common sense, salary increases as the rank in the job increases.
    - Generally, higher degree results in higher salary but there are little differences in salaries between None and high school degree and among degrees above University level.
    - Average salaries in education inudstry was the lowest while salaries in oil was the highest across all kind of job types.
    - Salary showed stronger increment with the years of experience when it was averaged than raw data.
    - Also, salary by distance between workplace and metropolitan area showed stronger negative correlation when it was averaged.

![alt text](https://github.com/hyj-main/portfolio_salary_pred/blob/master/image/1_4.png)

![alt text](https://github.com/hyj-main/portfolio_salary_pred/blob/master/image/1_7.png)

![alt text](https://github.com/hyj-main/portfolio_salary_pred/blob/master/image/1_9.png)

![alt text](https://github.com/hyj-main/portfolio_salary_pred/blob/master/image/1_10.png)



## 4. Feature engineering
* a. Cateogrical features
    - Job type and degree were label-encoded with an increasing order in EDA part(e.g. 1 for Janitor, 8 for CEO)
    - One-Hot-Encoding for major and industry
* b. Numerical features
    - Maximum salary by major
    - Maximum salary by industry
    - Average salary by years of experience
    - Average salary by distance from metropolitan area
    - Standardization of all numerical features
* c. Target: Data transformation
    - Square root transformation for target variable to make it closer to normal distribution


Mainly, not having a major is highly correlated with low levels of degree (no degree or high school), which is also correlated with becoming a janitor. 

## 5. Model development
This is a summary of model development. To see the detailed version, please click the following link.

[Model development Jupyter Notebook](https://github.com/hyj-main/portfolio_salary_pred/blob/master/salary_pred_modeling_optuna.ipynb)


* a. Baseline model with random forest that did not go through feature engineering showed an MSE of 440.40.
* b. Random forest model with feature engineering showed an MSE of 364.
* c. XGBoost regression with feature engineering showed an MSE of 356, which was the best model.

## 6. Conclusion

![alt text](https://github.com/hyj-main/portfolio_salary_pred/blob/master/image/1_8.png)


Overall, job type, the maximum salary in each industry and the degree were the three most important features to predict salary.
1) Job type level was the dominantly significant feature. Regardless industry and degree, salary increases as the job hierarchy increases. 
2) Maximum amount of salary by each industry was the next important feature in predicting salary, which increased from education to finance and oil. Generally, serving jobs can be regarded as the lowest paying job, but it is notable that companies in education industry tend to pay even less than companies in service. Industries related with economic impact such as finance and oil were the ones that pay highest amount of salaries. It indicates people generally are more affected by industries that are directly related with economical factors. 
3) Academic degree level was the third important feature perhaps because the average salary above university level did not show clear difference. For example, it is a common sense to assume PhD would earn more than MS and Bachelor's degree. However, that was not the case in this job posting data. Whether graduated university or not shows clear difference in salary.

Therefore, when employers want to set up a baseline for a salary for job posting, they can consider job type, industry and whether potential candidates gruadated university. 
