# Guess the Calories 🤤
Our exploratory data analysis on this dataset can be found [here](https://elijahzyp.github.io/recipes_ratings/).

### Framing the Problem

​		For our project, our objective is to estimate the **calories** of each recipe through feature engineering and modeling techniques. 

> **Prediction problem**: Predict calories of recipes

​		This is a regression problem. We selected calories as a significant variable for recipes because, if accurately predicted using other parameters, it can provide users with a straightforward guide for recipe selection. To forecast the numerical value of calories, we have opted for the DecisionTreeRegressor method to get a regression. We picked this model for our project because we are predicting a continuous numerical value and DecisionTreeRegressor can capture linear and non-linear relationships and is also robust to outliers. At this time, we know 22 other features including sugar, total fat, sodium, average rating, etc. To assess the accuracy of our model, we employ the R-squared metric instead of root mean squared error (RMSE), as it effectively evaluates the model's fit and its ability to explain the observed data. That is because RMSE cannot directly show the goodness of the model in a standard scaling (e.g. 0 to 1).



------



### Baseline Model

​		Our baseline model uses two features, which are published_year and total fat (PDV), to predict the calories. Among three features, one feature (total fat (PDV)) is quantitative and one feature (published_year) is categorical and ordinal. For feature engineering, we used OneHotEncoder() to encode the published_year feature. To avoid multicollinearity, we used the drop = “First” parameter, which drops the first column from the result of the encoding. Finally, we used DecisionTreeRegressor with a max_depth of 5 to fit the model and make a prediction. The hyperparameter max_depth is set to 5 randomly and we will tune it in the later section when we build our final model.

​		We test this baseline model on both training data and testing data so we can see how our model performs on unseen data. In the end, we have an R-squared value of 0.7947850043984259 for training data and 0.6751961883585864 for testing data. Generally, We think that this is a fair model that can make an okay prediction. Since R squared value is from 0 to 1 with higher being better, our model is not the worst model we can have but there are definitely improvements that we can make to get a better model.



------



### Final Model

​		In our final model, we decided to add more features that are related to calories. We decided to add sugar to the model to help improve R squared value. That is because high sugar is usually associated with high calories. Hence, we add this quantitative feature to our model. Besides sugar, we also added healthiness (whether each recipe is healthy or not based on a standard we developed using the amount of protein and carbohydrates in that recipe) as one of our features. Healthiness is a categorical binary feature (True or False: True for healthy; False for not healthy). We think this feature is good for our prediction because high calories can often be associated with “not healthy”, so this feature will help us better predict the calories of a recipe. Another feature we added is the number of ingredients for each recipe. We think this feature will help with our prediction because the more ingredients a recipe has, the higher chance it will have a higher calorie value since there are many different ingredients. This is a discrete numerical value. 

​		For our final model, we still used the DecsionTreeRegressor for the reasons we stated in the baseline model section. However, to find the optimal value for the hyperparameter, max_depth, we use GridSearchCV (K-Fold Cross Validation) with a cv of 5 (5 folds) to figure out the best hyperparameter, which is 8 at the end. With a max_depth of 8 for the DecisionTreeRegressor in our model, we got an R-squared value of 0.907312276504803 for our training data, and 0.8486574302087885 for our testing data. Since a higher R-squared value means that a higher proportion of the variation in the recipe’s calories can be explained by the regressors, our final model with a higher R-squared value for both training and testing data has improved from our baseline model. 



------



### Fairness Analysis

​		For our fairness analysis, we will look at two different groups of recipes. One of which is recipes that were published before 2012 (old recipes), and the other is recipes published after 2012 (new recipes). The two different groups are old and new recipes. Our evaluation metric will be the R-squared values. The null hypothesis is that our model is fair; the R-squared value for the old recipes and the new recipes are the same, and any differences are due to random chance. Our alternative hypothesis is that our model is unfair; the R-squared value for old recipes is lower than the value for new recipes. We choose the difference in the R squared value for both groups (old - new) to be our test statistic and the significance level is 0.05. The resulting p-value is 0.26, which is greater than the critical value of 0.05. Thus we fail to reject the null hypothesis that our model is fair. Our conclusion is that there is not clear sign that our model is unfair for old and new recipes but it is not certain to say that our model is 100% fair. 



<iframe src="assets/fairness_test.html" width=800 height=600 frameBorder=0></iframe>





