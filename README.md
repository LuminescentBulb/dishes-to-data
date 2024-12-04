# Dishes to Data
By Darren Dong & Taemin Kim
# Introduction
In this project we examine over 80,000 recipes and ratings posted on the website [food.com](https://food.com) since 2008. 
In particular, the recipes dataset contains most importantly the name, ingredients, nutritional values, description and the steps, which we will use to answer
our central question about these recipes and how they relate to ratings by people, since, after all what is a recipe without a good rating?
In that regard, the separate ratings dataset provides us the recipe ID to which the ratings are about as well as a numerical rating and their justification in text.

In order to answer our question we will first clean and explore the data. Then we will analyze how our data cleansing potentially affected the data.

Once the data is cleaned and explored, we will seek to answer the central question regarding this dataset. That is, which nutritional value, among 
calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV) have the greatest effect on the rating of a recipe? With that we will build a model that predicts ratings using those individual nutritional values.

This is important since people might want to know what nutritional value corresponds to better ratings among people and how they may want to adjust their recipe in order to receive higher ratings. This will be also good for public health officials in understanding what balance of nutrients appeal to people. 

There are two original datasets, `Recipes` and `Ratings`, both of whose columns are listed below:

**Recipes**

| Column | Description |
| --- | --- |
| `'name'` | Recipe name |
| `'id'` | Recipe ID |
| `'minutes'` | Minutes to prepare recipe |
| `'contributor_id'` | User ID who submitted this recipe |
| `'submitted'` | Date recipe was submitted |
| `'tags'` | Food.com tags for recipe |
| `'nutrition'` | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| `'n_steps'` | Number of steps in recipe |
| `'steps'` | Text for recipe steps, in order |
| `'description'` | User-provided description |





**Ratings**

| Column     | Description|
| --- | --- |
| `'user_id'`  | User ID|
| `'recipe_id'` | Recipe ID |
| `'date'`     | Date of interaction |
| `'rating'`   | Rating given |
| `'review'`   | Review text |


# Data Cleaning and Exploratory Data Analysis
We will first merge the two datasets into one and clean the dataset for further analysis.
## Cleaning
1. We start by left merging the `Recipes` and `Ratings` datasets by `contributer_id` and `user_id` in  `Recipes` and `Ratings` respectively. The resulting dataframe is called `raw_food`.

2. We then fill all ratings of 0 with `np.nan` since they are not valid ratings in a 1-5 scale and thus should be ignored in future calculation using ratings.

3. We then find the average rating per recipe as a Series as `avg_rating`.

4. We inner merge the resulting Series back into the `raw_food`, both on `recipe_id`. 

5. We then drop duplicate columns with duplicate `recipe_id` so we only have one `avg_rating` per recipe.

6. We then drop the individual `rating` since that data will not be necessary for further analysis.

7. We then dropped all rows missing `avg_rating` values, for reasons we will explain in our missingness analysis.

8. Finally, we split the nutrition value into its own separate columns `calories (#)`, `total_fat (PDV)`, `sugar (PDV)`, `sodium (PDV)`, `protein (PDV)`, `saturated_fats (PDV)`, `carbohydrates (PDV)` and drop the `nutrition` column. 

9. From the cleaned dataset we keep only the relevant columns which are `name`, `minutes`, `n_steps`, `n_ingredients`, `avg_rating`, `calories (#)`, `total_fat (PDV)`, `sugar (PDV)`, `sodium (PDV)`, `protein (PDV)`, `saturated_fats (PDV)`, `carbohydrates (PDV)`

The first few rows of the dataframe is shown below:

| name | minutes | n_steps | n_ingredients | avg_rating | calories (#) | total_fat (PDV) | sugar (PDV) | sodium (PDV) | protein (PDV) | saturated_fats (PDV) | carbohydrates (PDV) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | ---  | --- | --- |
| 1 brownies in the world best ever'  | 40 | 10 | 9 | 4.0 | 138.4 | 10.0 | 50.0 | 3.0 | 3.0 | 19.0 | 6.0 |
| 1 in canada chocolate chip cookies | 45 | 12 | 11 | 5.0 | 595.1 | 46.0 | 211.0 | 22.0 | 13.0 | 51.0 | 26.0 |

### Missingness Analysis
Since missing average ratings would mean the recipes with missing ratings wouldn't be used, we had to make the choice of how to handle the missing values. 

We decided to assume that ratings were missing completely at random since it didn't really make sense to assume that it was due to any of the other columns. It seemed more of a problem that they failed to be presented to recipe viewers in general, perhaps due to being a duplicate, or generally unknown. 

Since they are missing completely at random, we decided to simply drop the missing values, which had a mostly insignificant effect on the total data since there are more than 80,000 rows, of which only around 4,000 rows had missing average rating values.  

## Exploratory Data Analysis
### Univariate Analysis
We started our exploration by first exploring the distribution of one of the nutrition variables: calories.

<iframe
  src="assets/calories.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

In the above plot showing the distribution of calories of recipes, with outliers above 3000 calories removed, we see a generally right skewed distribution with a center around 200 calories. This gives us some early insight that most well-liked recipes are probably going to share the same trend of being in this middle range of calories. 

We then wanted to explore the distribution of the average ratings themselves:

<iframe
  src="assets/avg_rat.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

In the above plot showing the distribution of average rating, we see an overwhelming number of reviews are in the 4-5 range, which casts a certain doubt on the quality of the data. Regardless, this will give us some clue as to what are the recipes that are so bad that they acquired a low score despite a high proportion of 5's

### Bivariate Analysis

We then conducted some bivariate analyses to visualize if there were any apparant relationships between variables.
First we examined the effect of calories on average ratings:

<iframe
  src="assets/rat_vs_calories.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

In the above scatter plot of calories to average rating, we see that there is a huge lump of points that are focused primarily around low amount of calories and a high rating of 5. There is still an unclear association, since lower ratings tend to have lower calories too. This may hint at other factors in play that affect the rating. 

To explore more factors, we decided to examine a non-nutritional variable, the amount of steps in the recipe itself:

<iframe
  src="assets/rat_vs_steps.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

In the above scatter plot of the number of steps to average rating, we see yet again that there is a huge lump of points that are in the lower amount of steps but in the higher ratings. However this is probably a reflection of the trend that there are more ratings that are near 5, and points us toward another variable that may be affecting this association. 

### Grouping
We then decided to try analyze if there were any interesting relationships resulting by binning the average ratings and seeing their mean nutritional values. 

| binned_ratings_label   |   calories (#) |   total_fat (PDV) |   sugar (PDV) |   sodium (PDV) |   protein (PDV) |   saturated_fats (PDV) |   carbohydrates (PDV) |
|:-----------------------|---------------:|------------------:|--------------:|---------------:|----------------:|-----------------------:|----------------------:|
| 1-1.5                  |         347.15 |              27.5 |          44.5 |           16.5 |              15 |                   34.5 |                  11.5 |
| 1.5-2                  |         319.4  |              20   |          25   |           13   |              17 |                   23   |                  10   |
| 2-2.5                  |         263.8  |              14   |          27   |           14   |              16 |                   16   |                   9   |
| 2.5-3                  |         315.1  |              20   |          24   |           14   |              20 |                   22   |                   9   |
| 3-3.5                  |         293.1  |              17   |          24   |           13   |              17 |                   18   |                   9   |
| 3.5-4                  |         315.2  |              20   |          22   |           15   |              21 |                   21   |                   9   |
| 4-4.5                  |         306.85 |              19   |          22   |           15   |              20 |                   20   |                   9   |
| 4.5-5                  |         301.5  |              20   |          23   |           14   |              17 |                   21   |                   8   |

Remarkably, a lot of the ratings seemed to share the same nutritional values. However, there was a bit of a contrast with the lowest rated bin in that the 1-1.5 bin had higher calories, fat, sugar content than all the other ratings. This gave some insight as to what kind of nutritional figure people were looking for in recipes. 

# Framing a Prediction Problem
Our model will try predict the rating of a recipe using linear regression.
The values we will have available at the time of prediction are the nutritional values, the steps and the ingredients required for the recipe. Using `n_steps`, `n_ingredients`, `calories (#)`, `total_fat (PDV)`, `sugar (PDV)`, `sodium (PDV)`, `protein (PDV)`, `saturated_fats (PDV)`, and `carbohydrates (PDV)` we wish to predict the `avg_rating`. Since rating is the value that recipes are evaluated by people who use them, it will offer the best insight as to the value of a recipe to the user. 

The metric we will use to evaluate the model are mean squared errors and R^2 given that the MSE offers us clues as to how big the error is and R^2 will give us clues as to how unfit the model is to the data. 
# Baseline Model
We created a regression model with 5 degree polynomial features using the following features: number of steps, number of ingredients, calories, total fat, sugar, sodium, protein, saturated fats, and carbohydrates, all of which are quantitative. These features would be used to predict the average rating of a recipe, giving an insight into what kind of nutrients people prioritize in their recipes as well as in simplicity. 
Given the quantitative nature of all our features, we did not have to encode any features.

However, the performance of our model was disastrous, with a mean squared error of 2,357,910.16 on the test set and a R^2 value of -5,774,359.17. 
We reckon that the model overfit to the training data too much, as the training mean squared error was low at a value of 0.39. 
# Final Model

Given the rather disastrous performance of our baseline model, we decided to look to what new features and regression methods we could use 
in order to improve the performance of our regression model. 
After some investigation we decided to create a few more features. 

We created a new feature by applying np.log1p to every feature to transform them. The general association between the features and rating seemed to 
resemble the bottom left quadrant of the Tukey Mosteller Bulge Diagram, so we decided that applying a log to the features would linearize it better.
Another feature we created are all the polynomials raised to the power of 2, a hyperparameter selected by GridSearchCV. We believed that this polynomial feature 
would help us capture non-linear relationships better, since none of the associations were very linear naturally. 

In our model,  we also used RobustScaler to diminish the effects of outliers from our selected features and to regularize them. 
We then use ridge regression to alleviate the overfitting issue we had in the baseline model.
To find the optimal parameter for both the degree of the polynomials as well as the alpha value of the ridge regressor, we used GridSearchCV,
which chose a degree of 2 and a ridge alpha value of 100. 

The final model certainly improved over the baseline model. It had an MSE of 0.4073 and R^2 value of 0.0023 on the test set. 
The higher values on both metrics indicate a performance increase!

### Extras
