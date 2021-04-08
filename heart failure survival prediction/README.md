
# Project Proposal

Heart related diseases are the number one cause of death worldwide, accounting for almost 31% of all deaths across the globe. While heart failures can be prevented through strict diet and healthy life styles, the question we must also ask is: Given certain conditions about someone, how likely is it the person to survive from a heart failure; and what contributes the most in determining whether the person survives or not?

In order to predict the mortality rate caused by heart failures, dataset consisting informations of 299 patients who suffered a sever heart failure was gathered from https://www.kaggle.com/andrewmvd/heart-failure-clinical-data. We will be taking a deep analysis of how each features contribute to the mortality rate, and utilize various tree based models to evaluate the likelihood of survival for this classification problem.



## Data Wrangling

The dataset provided from the website was very clean to use with no missing values or entries that are seemingly erroneous, most of the data wrangling simply consisted of checking to make sure that there were no erroneous entries.

![image](https://user-images.githubusercontent.com/81454133/113958175-3d33a880-97e6-11eb-93ea-b9087e9189a5.png)


After careful research on what is considered to be acceptable range for features such as serum sodium levels and creatinine phosphokinase levels, there seemed to be no reasons to suspect that any of the entries were erroneous.

## Exploratory Data Analysis

Most of the EDA process consisted of trying to identify whether there are any association between the individual features and the resulting death events. In the case of 
categorical features such as gender, chi-squared contingency test was utilized in order to determine whether there exist an association between the feature and the death event. In the case of quantitative features such as age, patients were divided into groups who survived and who did not, and distribution of each group was examined.

![image](https://user-images.githubusercontent.com/81454133/113958731-32c5de80-97e7-11eb-964d-2b56a5475016.png)

Above shows the relationship between smoking and mortality rate; surprisingly, there seems to be no association between the two. A likely conclusion to be drawn here is that although smoking increases the chances of having a heart failure, it does not necessarily have an effect on whether the patient dies or not. Similarly, whether the patient had diabetes or not also seemed to have no impact on the mortality rate.

![image](https://user-images.githubusercontent.com/81454133/113959169-0363a180-97e8-11eb-9aba-ad048f603733.png)


Above shows the difference in age distribution between patients that survived and patients that did not, we can clearly see that the older the patient is, the likelier it is for the patient to die. Overall, the biggest take away from the EDA process was that having a high serum creatinine level or a high ejection fraction rate decreases the chance of surviving.

## Pre-Processing

Because the intention was to work with tree models, there was no need for the data values to be standardized. However, observations that consisted of data values greater than 3 standard deviations higher than the sample mean were removed since we were working with a relatively small dataset. Afterwards, dataset were split into 75% training sets and 25% test sets.

## Modeling/Results
First, an Entropy model and a Gini Impurity model were created without any hyperparameter tuning; it seemed that the model was simply overfitting to the training dataset, leading to poor evaluation metric scores.

![image](https://user-images.githubusercontent.com/81454133/113960314-eaf48680-97e9-11eb-8a43-fbf575f6a6e7.png)

### Random Forest Model
A Random Forest model was built using random search cross validation in order to tune the following hyperparameters: number of estimators, maximum features, maximum depth, and quality of split.

![image](https://user-images.githubusercontent.com/81454133/113960759-900f5f00-97ea-11eb-83d9-2953ee601f74.png)


Results above showed much greater improvement from the previous models, with the following feature importance graph:

![image](https://user-images.githubusercontent.com/81454133/113960864-ba611c80-97ea-11eb-8f29-501d1616bb77.png)


### Extreme Gradient Boosting Model
In search for an even better model, an XGBoost model was built using Bayesian Hyperparameter Optimization.

![image](https://user-images.githubusercontent.com/81454133/113960982-f4322300-97ea-11eb-9a92-0b73bb6bc378.png)

We can see that the results improved very slightly, producing the feature importance graph below:

![image](https://user-images.githubusercontent.com/81454133/113961036-0ad87a00-97eb-11eb-9ee0-b5c9e914d253.png)


## Conclusion

The most surprising finding from this project was that health conditions such as diabetes and high blood pressure, and unhealthy habits such as smoking does not affect the mortality rate as much as one would assume. Rather, it seems that the patient's serum creatinine level, as well as his/her ejection fraction rates are what contributes the most to the mortality rate. Having a high serum creatinine level puts a lot of pressure in the patient's kidney, and this must have a huge impact on whether the patient survives or not. Ejection fraction rate determines how much blood is pumped out of the heart for each contraction; based on the finding from EDA, having a high ejaction fraction rate must also contribute to having a lower chance of surviving from a heart failure.



