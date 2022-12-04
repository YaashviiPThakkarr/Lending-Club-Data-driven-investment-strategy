# Lending-Club-Data-driven-investment-strategy


In this project, we used Lending Club's rich data source of historical transactions to select which loans to invest inorder to maximise the customer's returns on those investments. 
We referred to Lendind Club's data - which is a Peer-to-peer lending platform. Peer-to-peer lending refers to the practice of lending money to individuals (or small businesses) via online platforms that match anonymous lenders (or investors) with borrowers. Lenders typically earn higher returns relative to investment products offered by banks. However, there is of course the risk that the borrower defaults on his or her loan.

The intermediary platform (such as Prosper or LendingClub) generates revenue by collecting fees on funded loans (from borrowers) as well as on servicing (from investors). The peer-to-peer lending industry in the United States started around 2006 and by 2012, LendingClub was the largest peer-to-peer lender based on issued loan volume and revenue. Peer-to-peer lending has been one of the fastest growing investments, with ∼16 billion in
loans reportedly originating from LendingClub by the end of 2015.

### Data
The dataset contains more than hundred features, including the following, for each loan:
1. Loan amount
2. Interest rate
3. Monthly installment amount
4. Demographic data - several additional attributes about the borrower (type of house ownership, annual
income, monthly FICO score, debt-to-income ratio, number of open credit lines, etc.)
5. Loan status (e.g., fully paid, default, charged-off)

High level data categories:

-*Borrower Profile metrics/demographics*: Metrics describing the profile of the borrower
annual_inc, purpose, zip_code, emp_title, home_ownership, acc_now_delinq, addr_state, verification_status, emp_length, emp_title, emp_length, fico_range_low, fico_range_high 
-*Temporal Metrics*: Metrics relating to time
mths_since_last_major_derog, mthsSinceLastDelinq, earliest_cr_line 
-*Loan metrics*: Metrics defining the loan status and loan terms. 
fundedAmnt, loanAmnt, term, url, effective_int_rate, installments, grade, sub_grade, title, purpose, loan_status, total_pymnt, last_pymnt_amnt 
-*Categorical Metrics*: Metrics which takes value from a fixed set 
Loan grade, employment Status, Loan Status, Verification status etc. 
-*Continuous Metrics*: Numerical metrics 
num_op_rev_tl,num_rev_tl_bal_gt_0, num_sats, etc


### Business Understanding - Phase 1

- As an investor, the investor will have to decide the following:
    -decide the total amount I am willing to invest in Lending Club. 
    -deciding on the loan portfolio on Lending Club (deciding between a risk-seeking v/s risk-averse strategy, preference for higher interest rates or lower risk,    
     duration) 
 - The investor's objective will be to maximize my profits (returns) and minimize the likelihood (and potential amount) of losses. 
     
 - The first decision of deciding the total amount of money an investor is willing to invest in Lending Club is a personal one. It will not be influenced by the available data from Lending Club. Rather, the investor's financial history, current investments in other financial vehicles, expected future expenses & cash flows will determine the total amount the investor is comfortable in investing. 
 
- Deciding the loan portfolio will be informed using the data from LendingClub, as information like the borrower’s credit score, loan grade, debt-to-income ratio will help me determine the risk attached with each investment, and thereby come up with an optimal investment that will maximize profits. 

-The different times during which different loans are defaulted will impact the investor's decision-making and downstream analysis. Specifically, he/she will face bigger losses if the borrower is expected to default immediately after approval. This will be any investor's least preferred option. Contrast this with loans that are defaulted close to the end of the loan term. In this case, investor's losses will be lesser/minimal and he/she would prefer it over the earlier scenario. Therefore, we think investor's decision will vary depending on whether he/she want my investment back sooner or if he/she has the bandwidth to wait for more gains. 

- The return on Investment (ROI) is the most important for an investor. Features like grade, loan period, loan amount, interest rate, total payment can help determine ROI. Each of these variables can be combined in an appropriate way to compute the “return” for a given investor. 

- We think the historical data would be helpful for making such decisions. Historical data would help us in identifying characteristics of loan defaulters without any bias due to the time period considered for the analysis. Eg: Considering only the recent data from 2020 might have more defaults due the job losses during covid, such biases can be avoided using historical data. 


### Exploratory Data Analysis, Feature selections and Outlier detection - Phase 2

- We can partition our historical data into 2 halves. One will help us infer meaningful relationships/patterns between the status of the loan and the different features we have (like the demographic data, borrower financial history etc.). We can then use this information to develop investment strategies and test them out on the second half of the data to understand if our hypothesis/“learnings” are helping us maximize our returns (“better” decisions) vs minimizing them (“worse” decisions).
 
- Using all of the provided features will not be a good idea as some features will have a very high number of missing values (like annual_inc_joint, hardship_type, sec_app_collections_12_mths_ex_med, sec_app_fico_range_high). Some features could be highly correlated with each other, introducing multicollinearity in our data (For ex: FICO score and total_il_high_credit_limit could be correlated with each other). Some of the provided features might not be available to our model at the time of testing. Therefore, it would not make sense to use those features at the time of training the model. Ex: total_pymnt will not be available at the time of testing. Also, features like loan_id (basically, all unique identification features) would not have a good predictive power as they are just unique identifiers. 

- Loan grade and Fico score are likely to be correlated as individuals with higher fico score indicates the individuals have a higher tendency to repay debts, hence are given higher loan grades. It is also possible that the loan_status and total_payment would be related to each other as for the same loan amount, if it has defaulted, it is more likely to have a lower total_payment than it would have if it was completely paid off. 

- We cannot use the features as they are, because features having missing values will need to be imputed, categorical features will need to be encoded (like using one-hot encoding), features would need to be scaled/standardized (using StandardScaler, MinMax Scaler etc.). 

- At first look, total_pymnt does not seem to be related to the loan status because a low total_pymnt cannot be compared across different loan listings. We also need information on the total loan amount (for a particular loan listing) to be able to deduce the status based on the total_pymnt. 
However, making a few assumptions like considering the same loan amount (keeping all other metrics fixed), it is also possible that the loan_status and total_payment would be related to each other. Ex: For the same loan amount, if it has defaulted, it is more likely to have a lower total_payment than it would have if it was completely paid off. 

-Total_pymnt refers to the total payment received to date. We should not use total_pymnt while training a model because total_pymnt will not be available to the model at the time of testing. Including this feature during training will lead to data leakage. 

#### Calculating Returns on Past historical loans 
![](https://github.com/YaashviiPThakkarr/Lending-Club-Data-driven-investment-strategy/blob/main/Case%20outputs/roi.png)

#### Calculating returns by loan grades 
![](https://github.com/YaashviiPThakkarr/Lending-Club-Data-driven-investment-strategy/blob/main/Case%20outputs/roibygrade.png)

The numbers are quite astonishing because we expected higher grade loans (like Grades E, F or G) to have a higher return. While this is true for the optimistic method, the returns seem to diminish for the return_PESS, return_INTa and return_INTb. 
According to the numbers in the table mentioned above, we would invest in loan grade A, since it gives a higher return for most of the methods we calculated.

#### Percentage of loans in each loan grade 
![](https://github.com/YaashviiPThakkarr/Lending-Club-Data-driven-investment-strategy/blob/main/Case%20outputs/percofloans.png)

#### Percentage of defaults by loan grade
![](https://github.com/YaashviiPThakkarr/Lending-Club-Data-driven-investment-strategy/blob/main/Case%20outputs/defaultbygrade.png)

Basically, 6.89% of Grade A loans default, while ~50% of Grade G loans default. As the loan grade increases, the percentage of defaulted loans also increases. (There is a positive correlation). 

#### Average intrest rate by grades
![](https://github.com/YaashviiPThakkarr/Lending-Club-Data-driven-investment-strategy/blob/main/Case%20outputs/avgint.png)

For loan grade A, the average interest rate is 7.2%, while for the average interest rate is 27%. Essentially, the interest rate increases monotonically as the loan grade increases. (It pays off higher to invest in a riskier loan). 

- *After outlier removal* : We observe that the data is much more balanced across the 3 loan statuses after we have removed outliers. Specifically, for the fully paid loans, none of the return variables (pessimistic and optimistic) are negative. Only the charged off loans have values greater than zero for recoveries.

- *Feature selection*: Total_pymnt refers to the total payment received to date. 'Recoveries' refers to the post-charge-off gross recovery. We should not use either of these 2 variables while training a model because 'total_pymnt' and 'recoveries' will not be available to the model at the time of testing. Including this feature during training will lead to data leakage

After phase 2, we saved a pickle file post data imputation and feature selections to move to the modelling phase.

### Modelling and Evaluation - Phase 3

- Model Training (Phase 3 specifically after initial cleaning & outlier removal):
1. Feature Engineering:
We created the outcome column (True if loan_status is either Charged Off or Default, False otherwise). Create a feature for the length of a person's credit history at the time the loan is issued. We then downsampled the data to 50,000 observations and split the dataset into 30,000 training samples and 20,000 test samples.
2. Evaluation:
For model validation, we use the fit_classification() function that considers various evaluation measures. We use f1 score as the performance evaluation criteria for testing various models in our consideration set.

We tried to experiment two different methods of splitting: 
1. Random splitting
The advantage of random data splitting procedure is that we can create as many different splits as needed, without impacting the ratio of the train-to-test samples. This helps in training our model for a wide variety of different train and test data combinations, thereby making the model robust. 

Random Splitting would work poorly when our data has high autocorrelation. That is, when there is similarity between observations as a function of the time lag between them. Now, for our dataset, there are features like annual_income that would gradually change over the years. For such datasets, special care must be taken to split the data by ensuring no information/data leakage between the training, test or validation sets, which may exaggerate the model performance. Random Splitting would not be able to avoid data leakage for such datasets.

2. Temporal Splitting
This splitting criteria is naturally suited for time-series forecasting. This method would work well in identifying if the train and test distributions are different and if the data is temporal in nature. If we do a random split of the data which is temporal in nature, we would get a model which would try to predict a test data point which chronologically came before the training point; which does not make a lot of sense. 

- Hyperparameters help is optimizing a model so that we get the optimal solution by minimizing the loss function. Hyperparameter tuning is important to increase the generalizability of any model. The hyper-parameters tuned for each of the model are as follows:

![](https://github.com/YaashviiPThakkarr/Lending-Club-Data-driven-investment-strategy/blob/main/Case%20outputs/hyperparameter.png)

We use weighted average f1-score as a performance measure because it gives a harmonic mean of precision and recall.

![](https://github.com/YaashviiPThakkarr/Lending-Club-Data-driven-investment-strategy/blob/main/Case%20outputs/f1scores.png)

Considering the F1 scores to evaluate the predictive power of the model, we can see that the predictive power of the model was 0.7235 when all the features were included, whereas it is 0.6948 when only “grade” was included in the L2 Logistic regression model. This indicates that the “grade” feature is actually quite predictive of the loan status.

#### We run the models again after removing the grades and interest rate. 

We use f1-score as  metrics to average the model performance for 100 random splits. These are the averages we obtain with standard deviation:

![](https://github.com/YaashviiPThakkarr/Lending-Club-Data-driven-investment-strategy/blob/main/Case%20outputs/f1scores2.png)

We can observe from the table that Random Forest  gives the highest f1-score (0.7509) in comparison to other models and has the least standard deviation (± 0.0031) indicating that it has higher generalizability power. This will help us in predicting the defaulters and non-defaulters more accurately. 

The best performing model is Random Forest. Our model’s scores agree with the grades assigned by LendingClub. We go about this by taking an average of the predicted defaulters, grouping them by the loan grade. We expect to see that the average of the predicted defaulters would increase as the loan grade changes from A to G. (since A is the safest loan grade, which is least likely to default).
Our observation is in line with our expectation. 

![](https://github.com/YaashviiPThakkarr/Lending-Club-Data-driven-investment-strategy/blob/main/Case%20outputs/grades.png)


#### Time Stability Test of Model

- f1_2010:  0.7852554428586984
- f1_2016:  0.7894034536891681


Hence, we can conclude that the performance of our model trained in 2010 performs slightly worse in 2017 than the model trained on more recent data in 2016. This is probably because our model might not be very stable because of the time-series nature of our data. For example, due to inflation, chances to default for the same amount of loan would be higher in 2017, than it would be in 2010. Therefore, models trained in 2016 would be able to better capture this than the models trained in 2010.

1. Regressing against all returns

![](https://github.com/YaashviiPThakkarr/Lending-Club-Data-driven-investment-strategy/blob/main/Case%20outputs/allreturns.png)

We can see that the performance of all the models is pretty poor. But Random Forest outperforms all other models. Moreover, the performance for M1 (optimistic) for RF is the best, while for M2 (pessimistic) is the worst.

2. Regressing against Default returns

![](https://github.com/YaashviiPThakkarr/Lending-Club-Data-driven-investment-strategy/blob/main/Case%20outputs/defaultreturns.png)

We can see that the performance of the models now is slightly better. Random Forest outperforms all other models. Moreover, the performance for M1 (optimistic) for RF is the best, while for M3 is the worst.

3. Regressing against  Non-Default returns 

![](https://github.com/YaashviiPThakkarr/Lending-Club-Data-driven-investment-strategy/blob/main/Case%20outputs/non-defaultreturns.png)

The performance of our models when regressed against non-defaulted loans is the best! Here again, Random Forest performs best (for the M2).

The BEST 1000 loans one can invest in are the ones coming from the best_investment function described in our jupyter notebook, which takes a weighted average of the possible returns for the test set. The maximum returns we can get for each respective return method are:
- ret_PESS: 0.1003976993332276
- ret_OPT: 0.23371219777471391
- ret_INTa: 0.6136832195692166
- ret_INTb: 1.8562623510695344

Default-Return gives the best possible solution as its returns are highest. We can pick the best 1000 loans by making predictions using the “Default-Return” strategy on the test data, sorting (in descending order) by the respective returns, and picking the top 1000. 

The “Default-Return” investment strategy performs the best as it gives the highest returns for 2 of the 4 return methods we are computing.. 
The random strategy performs poorly compared to the other strategies, it gives us losses when we calculate returns using M1. This could be because this model might tell us to invest in loans that are going to default. The data-driven strategies like Default, return and default-return perform much better than the random strategy. And the data-driven strategies give more conservative return estimates compared to the BEST model.

![](https://github.com/YaashviiPThakkarr/Lending-Club-Data-driven-investment-strategy/blob/main/Case%20outputs/IRR.png)

The above graph is plotted using “Default-Return” as the best strategy. We observe that the M1 return decreases with an increase in portfolio size. As the number of loans in the portfolio increases, the number of good loans to potentially invest in would decrease, making it difficult to match the return performance we get by modeling on a smaller portfolio of loans.










 





