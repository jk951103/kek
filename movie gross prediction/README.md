# Project Proposal
Would it be possible to predict how much a movie makes given certain information about the movie? This is certainly an interesting question that movie directors/fans may be concerned about; after all, the gross of the movie is arguably the sole determining factor of whether the movie was successful or not.

Using a dataset containing information about 5000 random movies from the imdb, we will try to predict the gross of the movie using the information that will only be available before the release of the movie.

![image](https://user-images.githubusercontent.com/81454133/117597989-6361a680-b10c-11eb-8920-10e7fdc9369a.png)


# Data Wrangling
The wrangling process began off by checking to see whether the budget and the gross of each movie is in local currency or in USD; this is very important as having a universal currency is absolutely essential. Average budget of Korean movies were used to compare with the average budget of US movies; this was because 1 USD is about 1000 KRW; which is a big enough discrepancy to see whether local currency is being used or not. 

![image](https://user-images.githubusercontent.com/81454133/117597737-d1f23480-b10b-11eb-84a2-27df098a2ff5.png)

As shown above, the average budget for Korean movies is about 300 times bigger than that of US movies; this is a clear indicator that local currency is being used(also an indicator that US movie market is about 3 times bigger than that of South Korea's).

Judging from the fact that the conversion rate for each country's currency to USD varies continuously, and that it is not known when the gross of each movies were calculated; it is virtually an impossible task to convert every local currency into USD. We will only consider movies that have been made in the US, as a vast majority(3807 out of 5000) of the movies in the dataset are from the US.


Many other features shown below were also removed; with reasons stated next to each removal process.
![image](https://user-images.githubusercontent.com/81454133/117598345-3c57a480-b10d-11eb-939a-2038173bc57b.png)


Next, the number of missing values were calculated:
![image](https://user-images.githubusercontent.com/81454133/117598418-64df9e80-b10d-11eb-9039-87d6f652a01c.png)

Because gross and budget are the most frequent missing values, every observation of with missing values will be dropped in order to not jeopardize the predictability of the model.

Next, which is perhaps the most important step of this data wrangling process; is to adjust the budget and the gross of each movie to current values; it would not make sense to say that a movie that made a million dollars in the 1930s performed equally as well as a movie that made a million dollars in 2016. the CPI library was used for this process.

![image](https://user-images.githubusercontent.com/81454133/117598742-023ad280-b10e-11eb-82c7-5ef84f1a328a.png)

Next, Genres were one-hot encoded for modeling purposes.

![image](https://user-images.githubusercontent.com/81454133/117598895-55ad2080-b10e-11eb-9ad3-35ae19a1802c.png)

Genres that did not appear in the observation were removed.

Lastly, GDP Growth rate for each year was added in as a feature to serve as an indicator for the economic well-being at the time.

![image](https://user-images.githubusercontent.com/81454133/117599072-bb99a800-b10e-11eb-8bd5-3a9879d3ef3e.png)
![image](https://user-images.githubusercontent.com/81454133/117599106-cce2b480-b10e-11eb-870e-3abf2d2270e6.png)


