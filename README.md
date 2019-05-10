# DS Mod 3 Project
Predicting whether customers of a retail food company would or wouldn't be converted on the final campaign using data provided by Rodolfo Saldanha [I'm an inline-style link](https://www.kaggle.com/rodsaldanha/arketing-campaign#ml_project1_data.xlsx)

## Methodology
* Target: Response i.e. whether or not customer was converted on final campaign. Either a 1 for yes or 0 for no.
* Features: birth year, education level, maritial status, annual income, number of children and teenagers in the home, number of days since last purchase (recency), amount spent on several different types of products over past two years, number of purchases through different channels, whether or not they were converted on five previous campaigns.

## Challenges
* Massive class imbalance: only about 12.5% of dataset had response of 1
* Not real data: This set was clearly created by a user and there were some inconsistencies and confusing aspects that needed to be cleaned.
    * Additionally, I couldn't use any domain knowledge or perform any research to gain more intuition on the data because of this.
* Large amount of variance, skewness and zero values in data

## Feature Engineering
* Used log transformations on skewed data to rein it in, but wasn't able to account for zero values, which made up about 50% of the data as seen below. Later excluded some, which took it down to 35%.
##### Before
![image-1](charts/variance_skewness_example.PNG)<br>
##### After 
![image-2](charts/variance_skewness_example_after_log.PNG)<br>
* Created spends on different categories as proportions of income
* Created dummy variables for all categorical and discrete data
* Ultimately had 47 features

## Machine Learning Model Building
* Before actually using any models, data was scaled using the Min Max Scaler and target was oversampled on training using SMOTE method.
* Also tested out creating polynomial features, but having over 1200 vs. 47 actually didn't really make any difference
    * Select k best was used to reduce the number, but ultimately excluded since it was found that it didn't help the models
* Dummy classifier set baseline accuracy at 85%, demonstrating the massive class imbalance to be working against

#### Logistic Regression
| Baseline Scores | Tuned Scores | 
| --------------- | ------------ |
| Accuracy: 80.5% | Accuracy: 81.4% | 
| F1 Score: 0.540 | F1 Score: 0.548 | 

#### K Nearest Neighbors
|Baseline Scores | Tuned Scores |
| Accuracy: 69.0% | Accuracy: 78.8% |
| F1 Score: 0.335 | F1 Score: 0.346 |
| 

#### Random Forest
| Baseline Scores | Tuned Scores |
| Accuracy: 87.1% | Accuracy: 87.1% |
| F1 Score: 0.523 | F1 Score: 0.555 |

#### XG Boost
| Baseline Scores | Tuned Scores |
| --------------- | ------------ |
| Accuracy: 85.1% | Accuracy: 88.0% |
| F1 Score: 0.520 | F1 Score: 0.550 |

## Conclusions
* Ultimately decided on the XG Boost as my final, improving accuracy by a whopping 3%!
* A major caveat for this model is that there is a potential for overfitting as it the training data almost perfectly, but this could be due to class imbalance
* All models including XG Boost seemed to perform quite well on the negative outcomes, but was only able to correctly predict positive ones about 3 out of 5 times as you can see in the confusion matrix
![image-3](charts/XG_Boost_Confusion_Matrix.png)<br>
* Previous acceptances on other campaigns were most signficant indicators of wehter they were converted on the final campaign or not as seen in the below chart of feature improtances
![image-4](chart/feature_importance.png)<br>

## Next Steps
* Models seemed to be rather complex. Would experiment further with trimming as parameters like max depth and number of estimators seemed high
* Examine correlation between features, not just the features and target variables to see if there were any redundancies I could have accounted for
* Dive deeper into the potential of overfitting. Just dropping features proportionally made both the training and testing scores worse, and did not make them more even
