# Prediction of Bicycle Rentals in Washington, D.C.

The current study deals with the exploration and preparation of a dataset concerning bicycle rentals in Washington D.C, in order to predict the total number of rental bicycles per hour. The dataset is structured as follows:
* **instant:** record index
* **dteday:** date
* **hr:** hour (0 to 23)
* **weathersit:** Weather situation
* **temp:** Normalized temperature in Celsius. The values are divided to 41 (max)
* **atemp:** Normalized feeling temperature in Celsius. The values are divided to 50 (max)
* **hum:** Normalized humidity. The values are divided to 100 (max)
* **windspeed:** Normalized wind speed. The values are divided to 67 (max)
* **casual:** count of casual users
* **registered:** count of registered users
* **cnt:** count of total rental bikes including both casual and registered. This is the target to predict.

This study has effectively prepared a dataset for its prediction with a model, which in turn has been selected after a thorough search within linear and non-linear algorithms, simultaneously evaluating their performance with different parameter choices. 
With respect to the exploratory data analysis, the following can be extracted:
* 'weathersit', 'temp', 'atemp', and 'hum' all present NaN values, at different rows
* 'weathersit' is the only categorical feature, with 4 possible categories
* 'temp' and 'atemp' have a fairly centered distribution, whereas 'cnt' and 'windspeed' are right-skewed and 'hum' is left-skewed.
* 'windspeed' and 'cnt' have outliers for large values of the variable, whereas 'hum' has outliers at 0.
* 'atemp' and 'temp' are very strongly correlated with each other, at the same time being correlated significantly with 'cnt'. 'hum' is negatively correlated with 'cnt', and 'hr' follows a non-monotonic relation with 'cnt', with a peak in bicycle rentals around mid-afternoon.

With regards to the data engineering of the dataset, the following steps have been followed:
* From 'dteday', columns indicating the season, whether the day was in the weekend, and whether the day was a holiday, have been generated.
* For 'weathersit', four intuitive categories have been defined, imputing NaN values based on the most common weather condition of each month.
* For 'hr', the numerical value has been re-scaled to the range from 0 to 1, the hours with daylight have been labeled, as well as rush hours.
* For 'hum', outliers at 0 and NaN values have been imputed with the average of each month, and the variable has been normalized to reduce its skweness.
* For 'windspeed', outliers for large values and NaN values have been imputed with the average of each month, and the variable has been normalized to reduce its skweness.
* For 'atemp' and 'temp', both variables have been combined into 'wtemp', imputing outliers within one variable with the corresponding value of the other.
* For the target variable 'cnt', a normalization has seen fit to reduce its skewness, although this transformation is not included in the final preprocessing pipeline.

In order to select and train an adequate model:
* A simple linear regression, a k-nearest neighbors regression, a random forest regression, and a gradient boosting regression have been attempted with default hyperparameters. The model with the best R2 in this preliminary examination was the random forest regressor.
* Based on the three best-performing models, an extensive grid search has been performed, once again confirming the best performance of the random forest regressor.
* As a result, the most relevant hyperparameters of the `RandomForestRegressor` have been further examined, revealing a flawed selection of hyperparameters in the previous grid search. The optimum model corresponded to `n_estimators = 50`, `max_depth = 10`, and `min_samples_split = 10`.
* Predicting with this optimum model resulted in `R2 = 0.856`, with a satisfactory prediction plot.

To further improve this model, the following alternatives can be explored:
* Could all numerical variables be completely normalized, such that their distributions are centered at 0? This would imply normalizing 'wtemp', and not re-scaling any of the other numerical variables ('hum', 'windspeed', etc.) after their normalization.
* Could other more complex models be used to predict 'cnt' with greater success? Would a neural network return a better prediction?
* Could one split the dataset into multiple sub-datasets, training a different model to each split? For example, one could have a model for holidays and another for work-days.
* Could PCA be used to fit a model based on a smaller number of transformed, combined features, resulting in a better model?
