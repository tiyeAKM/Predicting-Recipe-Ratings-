# Analyzing the Relationship between the Ratings of Recipes and the Cook Time 

Author: Tiye Kurtz-Miott

## Overview

This is a project for DSC80 at UCSD and this is the Jekyll GitHub Pages Site which includes data analysis for recipe cook times, ratings, and more. 

## Introduction

There are several questions of interest for this dataset, however the main focus of is centered around the affect that cook time has on the recipe rating. For those who cook often, the best meal is sometimes created by balancing the ingredients, end result, and the time it takes to make it. Therefore, if the length of the recipe cook time has a negative or positive affect on the rating people give the recipe, it may allow future cooks to easily search and select the perfect recipe for them. Additionally, it could give valuable feedback to consistent contributors to Food.com as they might priortize different features of their recipe (e.g. minutes/cook time).


### Questions of Interest:
- Does the amount of ingredients affect the rating or cook time?
- Is the recipe rating is higher on average when the cooking time is low?
- Is the number of reviews correlated to the rating at all?

Looking at the first dataset, recipes, it contains a subset of the recipes from Food.com, we have 83782 rows × 12 columns and in the second dataset, ratings has a subset of Food.com's recipe reviews, there are 731927 rows × 5 columns.

The columns relevant in this analysis include: 'rating', 'review', 'minutes', 'n_ingredients', 'n_steps', 'description', 'ingredients', 'contributor_id', and 'tags'.

| Column             | Description                                                                                                                                                                                       |
| :----------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `'name'`           | Recipe name                                                                                                                                                                                       |
| `'id'`             | Recipe ID                                                                                                                                                                                         |
| `'minutes'`        | Minutes to prepare recipe                                                                                                                                                                         |
| `'contributor_id'` | User ID who submitted this recipe                                                                                                                                                                 |
| `'submitted'`      | Date recipe was submitted                                                                                                                                                                         |
| `'tags'`           | Food.com tags for recipe                                                                                                                                                                          |
| `'nutrition'`      | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| `'n_steps'`        | Number of steps in recipe                                                                                                                                                                         |
| `'steps'`          | Text for recipe steps, in order                                                                                                                                                                   |
| `'description'`    | User-provided description                                                                                                                                                                         |
| `'ingredients'`    | Text for recipe ingredients                                                                                                                                                                       |
| `'n_ingredients'`  | Number of ingredients in recipe                                                                                                                                                                   |

The second dataset, `ratings`, includes the columns of:

| Column        | Description         |
| :------------ | :------------------ |
| `'user_id'`   | User ID             |
| `'recipe_id'` | Recipe ID           |
| `'date'`      | Date of interaction |
| `'rating'`    | Rating given        |
| `'review'`    | Review text         |

The first step is to combine these two datasets so that our analysis can address whether or not a recipe's length of time has any affect on the way the reviewer rates it. 


## Data Cleaning and Exploratory Data Analysis

1. To start of the  analysis, the two datasets of ratings and recipes are merged with a left join on the 'id' and 'recipe_id' column in order to begin the data cleaning process. 

After this first step, the merged_df will contain the 17 columns and 234429 rows 234429:

- | Column             | Description |
     | :----------------- | :---------- |
     | `'name'`           | object      |
     | `'id'`             | int64       |
     | `'minutes'`        | int64       |
     | `'contributor_id'` | int64       |
     | `'submitted'`      | object      |
     | `'tags'`           | object      |
     | `'nutrition'`      | object      |
     | `'n_steps'`        | int64       |
     | `'steps'`          | object      |
     | `'description'`    | object      |
     | `'ingredients'`    | object      |
     | `'n_ingredients'`  | int64       |
     | `'user_id'`        | float64     |
     | `'recipe_id'`      | float64     |
     | `'date'`           | object      |
     | `'rating'`         | float64     |
     | `'review'`         | object      |

1. Second, in the ratings column, '0' ratings were replaced with NaN because 0 is not a valid part of the rating scale and np. NaN represents the absence of a rating better. By replacing these values, we are able to maintain the integrity of the data and improve the accuracy of analysis.


1. To further analyze ratings, the column`'average_rating'` was added to merged_df. Now we can group the recipes by rating if needed and use this column as another possible feature to see if it influences any other features. 

1. The strings in columnns 'tags', 'description', 'steps', 'ingredients' were into actual lists in order to create columns for the number of tags and ingredients for each recipe. This will help us measure possible relationships between the length of these features and the length of time a recipe takes to cook.


1. Here, the number of tags in each recipe was added as the column 'n_tags' and  'n_ingredients' containing the number of ingredients in a recipe. 


#### Resulting DF 
These are the columns and descriptions of the cleaned dataframe, 

| Column                  | Description    |
| :---------------------- | :------------- |
| `'name'`                | object         |
| `'id'`                  | int64          |
| `'minutes'`             | int64          |
| `'contributor_id'`      | int64          |
| `'submitted'`           | datetime64[ns] |
| `'tags'`                | object         |
| `'nutrition'`           | object         |
| `'n_steps'`             | int64          |
| `'steps'`               | object         |
| `'description'`         | object         |
| `'ingredients'`         | object         |
| `'n_ingredients'`       | int64          |
| `'n_tags'`             | float64        |
| `'recipe_id'`           | float64        |
| `'date'`                | datetime64[ns] |
| `'rating'`              | float64        |
| `'review'`              | object         |
| `'average rating'`      | object         |



#### For Further Exploration of Data

We looked at the descriptive statistics for the merged dataset, as well as the most popular recipes and a histogram of the ratings in our dataset. Additionally, the  most common ingredients in highly rated recipes were found.

<iframe
  src="assets/rating-dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Univariate Analysis

Moving on to the Univariate Analysis of our data, in order to work with the 'minutes'data from our merged_df, the outliers needed to be removed as they create too much noise. Therefore, after finding the IQR of the mininutes column, the lower bound was -40.0 and the upper bound 120.0. There were 24291 outlier datapoints that are not included in the analysis. Extending the range of the data to twice that of the upper bound, our merged_df now includes the recipes that took at least 4 hours or less to make. This brings our new dataset to 221244 columns and 18 rows, and a boxplot of the distribution shows that the majority of datapoints lie below the 100 minute mark in recipe cook time. The average cook time appears to hover aorund 30 minutes long. 

<iframe
  src="assets/boxplot_recipe_minutes.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/histogram_cook_time.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


### Bivariate Analysis

In this section, we attempt to find out who has contibuted the most amount of recipes with the highest ratings, as well as look at the correlation between the number of tags, ingredients, steps, and minutes. To do this, a dataframe with the top contributor id's was found and sorted by average rating. The bar graph of our distribution shows that contributor id: 37449 was the person who contributed the most and whose recipes ended up with a higher rating on avergae. To look at the correlation between the features of the number of tags, ingredients, steps, and minutes, a heatmap was created which shows a positive relationship between the number of ingredients, number of steps, ingredients, and minutes. However, a negative correlation was found between those three variables and the number of tags. 

<iframe
  src="assets/top_recipe_contributors.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Interesting Aggregates

To further uncover patterns and insights within the merged_df, the relationship between number of ingredients and the average rating was analyzed with a line plot. Our graph shows that, perhaps surprisingly, as the number of ingredients rises so too does the rating of the recipe, with recipes with over 30 ingredients having a top rating of 5. In another analysis to understand the relationship between the number of ingredients, number of steps, and the average minutes, we created a heatmap to see the effect of any influences. This heatmap is quite different from the first and shows the relationship between the number of ingredients, number of steps, and the average minutes the recipe takes.

<iframe
  src="assets/avg_rating_ingredients.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/avgtime_heatmap.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


## Assessment of Missingness

We assessed `'rating'`, and `'review'`, from the merged dataset as they were found to have the most amounts of missing data.

### NMAR Analysis

Looking at the missingness of the columns in our merged dataset, the absence of ratings for a recipe could be NMAR values as reviewers might not have left a rating for a recipe they didn’t try or don’t like. Therefore, this column's missinngness value is directly related to the quality of the recipe’s quality, collaborator's steps/description, or the way the recipe turned out for user themselves. Under the same reasoning, if a reviewer hasn't given a recipe a rating, they might be more likely not leave a review. With these assumptions, we did further analysis to determine if recipes with more steps or ingredients tend to have more missing ratings.

### Missingness Dependency

Working with the missingness in the 'ratings' column, we found that there could be possible correlations between the values missing and the number of steps or ingredients in the recipes. The column of 'rating_missing' was addded to our main dataset, and any null values were dropped in the columns of 'n_steps', 'n_ingredients', 'rating_missing'. Moreover, the observed differences between recipes with ratings missing and ratings not missing was found to be . 

> Viewed the Missingness of the Ratings Column by the Number of Recipe Steps or Ingredients

**Null Hypothesis:** The missingness of ratings does not depend on the number of steps or ingredients in the recipe.

**Alternate Hypothesis:** The missingness of ratings does depend on the number of steps or ingredients in the recipe.

**Test Statistic:** The absolute difference of mean in the number of steps or ingredients of the distribution of the group with missing ratings and the distribution of the group without missing ratings.

**Significance Level:** 0.05

A permutation test was attempted  and the observed differences between recipes with ratings missing and ratings not missing were used as the test statistic. Unfortunately there was an issue running the Permutation tests, but during our testing, these statistics were observed:

Feature: rating
Observed Difference: 0.0000
P-value: 1.0000


Feature: n_steps
Observed Difference: 0.0624
P-value: 1.0000


Feature: n_ingredients
Observed Difference: 0.0624
P-value: 1.0000

## Hypothesis Testing

**Null Hypothesis:** There is no difference in the average ratings between recipes with low cooking times and those with high cooking times.

**Alternative Hypothesis:** The recipe rating is higher on average when the cooking time is low. 
**Test Statistic:** The difference in mean between rating of recipes with low and high cook times.

**Significance Level:** 0.05

<iframe
  src="assets/perm_test_hypothesis.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


#### Conclusion of Permutation Test

A permutation test was run in order to test if there was a correlation between the rating of the recipe and when the cook time is high or low. The median cook time for the recipes in our df was found to be 35 minutes, therefore we defined two groups and split the recipes into two groups based on low or high cooking time. The Observed Difference in Means was then calculated to determine if there is a significant difference in average ratings based on cooking time. In this case, our p-value shows the proportion of permutations where the difference in means is as extreme as or more extreme than the observed difference. Our Observed Difference in Means was found to be 0.0254312851082954 with a P-value of 0.0. Therefore, the interpretation of the test statistic is that on average, recipes with lower cooking times have ratings that are approximately 0.025 higher than recipes with higher cooking times.

Due to our p-value that is less than our reasonable significance level at 0.05, we reject the null hypothesis. There is statistical proof of a significant difference in average ratings between recipes with low cooking times and those with high cooking times, which supports out alternative hypothesis that recipes with lower cooking times have higher average ratings.


## Framing a Prediction Problem

The plan is to **predict the minutes of a recipe** which would be a **regression problem**. This prediction problem is going to be focused on predicting the amount of time a recipe takes based on the number of steps and rating of the recipe. In this case, linear regression is appropriate since the target variable (minutes) is continuous. There is a correlation between the rating given and the amount of time a recipe takes to make as we've seen above, so in this case the recipe time may be able to be deduced from the rating variable.

The Mean Absolute Error will be the test metric to evaluate the performance of our prediction problem as well as the Root Mean Squared Error (RMSE) which provides an error measure in the same units as the target variable.

## Baseline Model

#### Conclusions
Within the  baseline model, after the data points have been split into training and test sets, the linear regression model is trained on the dataset that is created from the 'n_steps', 'rating', 'minutes' columns. Then it's fit in order to make predictions on our target vairable 'minutes'.

The mean absolute error is 23.645888899054093 and the root mean squared error is 34.92699727881018. Our MAE at 23.6 and RMSE at about 34.93 suggests that the typical error in the model's predictions is about  23 to 34.93 minutes. This suggests that the model does capture the relationship patterns between the predictor variables of steps and rating and the target variable, cooking time, however, there is still a considerable amount of error in predictions.

## Final Model

For the final model, we are including the TF-IDF values of words from the columns of 'tags', 'description', and 'review', along with our initial parameters. To do this, we've createed a copy of our Merged_df and transforms values in 'tags' column in copied dataframe to string again. The goal is to view if the level of importance or "rarity" of the word in either the 'tags', 'description', or 'review' can be used to accurately predict the cook time of a recipe. We used the TfidfVectorizer() on these text features and a StandardScaler() on the numeric features we'll be testing. Our model was found with these test values below:

**Mean Absolute Error (MAE):** 14.236631375192053
**Mean Squared Error (MSE):** 644.2737784014391
**Root Mean Squared Error (RMSE):** 25.38254869790343

Therefore, our final model was found to perform slightly better than our baseline model and showing the typical error in the model's predictions to be off by 14 minutes or 25 minutes under the RMSE.

Grid Search to check for the optional hyper parameters was attempted but failed due to high computation time. Further plans for the model include modfiying the model for optimal computation times as well as adding the feature of checking key terms in all of the text feature columns. We found key terms to be words such as 'quick', 'easy', 'slow-cooked', 'gourmet', 'simple', 'few ingredients', 'one-pot', 'elaborate', 'marinate overnight'. In other words, the belief is that the inclusion of one or more of the key terms would lead our cook time prediction to be lower or higher based on the eky term. 


## Fairness Analysis

In the Fairness Analysis of the model, we attempted to compare recipes under or over 30 minutes. Unfortunately, the computation time of the final model interfered our ability to perform permuation readings, but essentially a model dataframe was created to split the recipes into two groups to test with. The final model is fit with each group and the test statistic of each groups RMSE and R^2 is computed. After one round of testing, the RMSE of group X (less preparation time) was 1.397 and for group Y (more preparation time) it was about 10.659. Interestingly, the R^2 was the same for both groups at -63.267 which unfortunately suggests a poor fit for both groups as the values are the same. As the RMSE for group X is significantly higher than group Y, this indicates that the model's predictions are much less accurate for Group X. In the end, this suggests that there is potential bias or unfairness in the final model's performance across different groups.

#### Group X:
Recipes with less than 30 minutes preparation time.

#### Group Y:
Recipes with 30 minutes or more preparation time.

#### Null Hypothesis: 
The model’s performance is the same for Group X and Group Y.

#### Alternative Hypothesis: 
The model’s performance is different for Group X and Group Y.

#### Test Metric:
RMSE and R^2

**Group X:** RMSE: 1.397992964986309, r^2: -63.26714048554339

**Group Y:** RMSE: 10.6598217593117, r^2: -63.26714048554339

If our permutation test was able to be conducted, our p-value would indicate if we could reject or accept the null at the 0.05 significance level.
