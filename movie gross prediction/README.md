# Project Proposal
Is it be possible to predict how much a movie would make before its release? This is certainly an interesting question that movie directors/fans may be concerned about; after all, the gross of the movie is arguably the sole determining factor of whether the movie was successful or not.

Using a dataset containing information about 5000 random movies from the imdb, we will try to predict the gross of the movie using the information that will only be available before the release of the movie.

![image](https://user-images.githubusercontent.com/81454133/117597989-6361a680-b10c-11eb-8920-10e7fdc9369a.png)


# Data Wrangling
The data wrangling process began off by checking to see whether the budget and the gross of each movie is in local currency or in USD; this is very important as having a universal currency is absolutely essential to train the model correctly. Average budget of Korean movies was used to compare with the average budget of US movies; this was because 1 USD is about 1000 KRW, which is a big enough discrepancy to see whether local currency is being used or not. 

![image](https://user-images.githubusercontent.com/81454133/117597737-d1f23480-b10b-11eb-84a2-27df098a2ff5.png)

As shown above, the average budget for Korean movies is about 300 times bigger than that of US movies; this is a clear indicator that local currency is being used(also an indicator that US movie market is about 3 times bigger than that of South Korea's).

Judging from the fact that the conversion rate for each country's currency to USD varies continuously, and that it is not known when the gross of each movies were calculated; it is virtually an impossible task to convert every local currency into USD. We will only consider movies that have been made in the US, as a vast majority(3807 out of 5000) of the movies in the dataset are from the US.


Many other features shown below were also removed, with reasons stated next to each removal process.
![image](https://user-images.githubusercontent.com/81454133/117598345-3c57a480-b10d-11eb-939a-2038173bc57b.png)


Next, the number of missing values were calculated:
![image](https://user-images.githubusercontent.com/81454133/117598418-64df9e80-b10d-11eb-9039-87d6f652a01c.png)

Because gross and budget are the most frequent missing values, every observation with missing values will be dropped in order to not jeopardize the predictability of the model.

Next, which is perhaps the most important step of this data wrangling process, is to adjust the budget and the gross of each movie to current values; it would not make sense to say that a movie that made a million dollars in the 1930s performed equally as well as a movie that made a million dollars in 2016. the CPI library was used for this process.

![image](https://user-images.githubusercontent.com/81454133/117598742-023ad280-b10e-11eb-82c7-5ef84f1a328a.png)

Next, Genres were one-hot encoded for modeling purposes. Genres that did not appear in the observation were removed.

![image](https://user-images.githubusercontent.com/81454133/117598895-55ad2080-b10e-11eb-9ad3-35ae19a1802c.png)


Lastly, GDP Growth rate for each year was added in as a feature to serve as an indicator for the economic well-being at the time.

![image](https://user-images.githubusercontent.com/81454133/117599072-bb99a800-b10e-11eb-8bd5-3a9879d3ef3e.png)
![image](https://user-images.githubusercontent.com/81454133/117599106-cce2b480-b10e-11eb-870e-3abf2d2270e6.png)

# Exploratory Data Analysis
Relationship between some of the seemingly important features and the gross of each movie were examined; but it seemed that there was no specific feature that had a 'gigantic' impact on the gross of the movie as one would assume.

![image](https://user-images.githubusercontent.com/81454133/117599410-65793480-b10f-11eb-95dc-80881c9eef6a.png)

For example, above shows the relationship between the budget of a movie and the gross of the movie; there is a moderate correlation at best.

An elementary attempt at seeing which genres contribute to the movies success was examined as shown below:

![image](https://user-images.githubusercontent.com/81454133/117599595-b721bf00-b10f-11eb-93e5-472702fb9ab5.png)

It seems like Documentary and Musical movies are the most profitable genre; Animation, Adventure, and Action movies cost the most to make.


Some conclusiosn to be made from the EDA process were that budget had somewhat of an influence in how much the movie would make(as one can easily assume), and that the popularity of the director and the actors did not have as big of an influence as one would assume.

# Pre-Processing
The 'color', 'language', and 'content_rating' features were one-hot encoded, as they are the only categorical features remaining in the dataset.

![image](https://user-images.githubusercontent.com/81454133/117600265-3e236700-b111-11eb-93e0-719f833a4e13.png)


Standard scaling process was skipped, as the intention was to mainly use tree based models. In the case that other regression methods were to be utilized later on, some other feature engineering process would have been done before rescaling the dataset.

# Modeling
## Default Random Forest Regressor

![image](https://user-images.githubusercontent.com/81454133/117600555-c1dd5380-b111-11eb-85c7-348d3e32d1a7.png)
![image](https://user-images.githubusercontent.com/81454133/117600579-cf92d900-b111-11eb-82e4-fe717b519507.png)

With the R-Squared value being around 0.4 and the Mean Absolute Error being too high, it seems necessary to tune some hyperparameters.

## Using Random Search Cross Validation

Grid space was set as below; 3 folds were used for the cross validation process.
![image](https://user-images.githubusercontent.com/81454133/117600818-53e55c00-b112-11eb-890c-33c58c2cd051.png)

New model was fit and used to predict, producing the results below:
![image](https://user-images.githubusercontent.com/81454133/117600906-82633700-b112-11eb-9f7f-1a41c13c1894.png)

Mean Absolute Error is still too high, and although the R-Squared score got a little better, the model seems to be overfitting.

### Handling Outliers
The new model was used to predict the gross of each movie and to find outliers; careful steps were taken to avoid data leakage.

![image](https://user-images.githubusercontent.com/81454133/117601087-fd2c5200-b112-11eb-8f9f-82604210d394.png)

113 of the observations above were removed in an attempt to increase the predictability of the model; new predictions were made without these outliers as shown below:

![image](https://user-images.githubusercontent.com/81454133/117601248-76c44000-b113-11eb-8c13-77d8bb5af35e.png)

Just from removing some of the outliers, we can see that the R-Square score went up significantly and that the model is not overfitting at all. However, the Mean Absolute Error still seems to be too high.

## Modifying Dataset
In an attempt to see whether the model can perform better using less features; only some notable features were used to predict the gross of the movie as shown below:

![image](https://user-images.githubusercontent.com/81454133/117601364-c4d94380-b113-11eb-8aa6-e5eb762f6d30.png)

We can see that the model is terribly overfitting, and that both the R-Squared score and the Mean Absolute Error got significantly worse.

Next attempt was to bring back the rating of the movie from the original dataset and include that as one of the features; this is justifiable in a sense that it is possible to have a group of audience rate the movie before the actual premier of the movie.

![image](https://user-images.githubusercontent.com/81454133/117601725-a758a980-b114-11eb-8f3c-aafcaca5ac03.png)

Random Search Cross Validation was used once again to find the new best model; and after only removing 62 outliers, the following results were obtained.

![image](https://user-images.githubusercontent.com/81454133/117601872-f0a8f900-b114-11eb-9f48-407909921993.png)

The Mean Absolute Error went down significantly from the previous models; and the model is not overfitting at all.

![image](https://user-images.githubusercontent.com/81454133/117601955-20f09780-b115-11eb-887a-87b6ef9e554b.png)

Feature importances and the scatterplot of Actual Gross vs Predicted Gross are shown above.






