## Introduction
In this project we examine over 80,000 recipes and ratings posted on the website [food.com](https://food.com) since 2008. 
In particular, the recipes dataset contains most importantly the name, ingredients, nutritional values, description and the steps, which we will use to answer
our central question about these recipes and how they relate to ratings by people, since, after all what is a recipe without a good rating?
In that regard, the separate ratings dataset provides us the recipe ID to which the ratings are about as well as a numerical rating and their justification in text

In order to answer our question we will first clean and explore the data. Then we will analyze how our data cleansing potentially affected the data.

Once the data is cleaned and explored, we will seek to answer the central question regarding this dataset. That is, which nutritional value, among 
calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV) have the greatest effect on the rating of a recipe? With that we will build a model that predicts ratings using those individual nutritional values and maybe more [EDIT THIS]. IF WE HAVE TIME USE TF-IDF TO FIND RECIPE GIVEN INGREDIENTS

This is important since people might want to know what nutritional value corresponds to better ratings among people and how they may want to adjust their recipe in order to receive higher ratings. This will be also good for public health officials in understanding what balance of nutrients appeal to people. 

There are two original datasets, `recipes` and `ratings`, both of whose columns are listed below:

---
layout: post
title: "Recipes"
---

| Column | Description |
| --- | --- |
| 'name' | Recipe name |
| 'id' | Recipe ID |
| 'minutes' | Minutes to prepare recipe |
| 'contributor_id' | User ID who submitted this recipe |
| 'submitted' | Date recipe was submitted |
| 'tags' | Food.com tags for recipe |
| 'nutrition' | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| 'n_steps' | Number of steps in recipe |
| 'steps' | Text for recipe steps, in order |
| 'description' | User-provided description |



---
layout: post
title: "Ratings"
---

| Column     | Description|
| --- | --- |
| 'user_id'  | User ID|
| 'recipe_id' | Recipe ID |
| 'date'     | Date of interaction |
| 'rating'   | Rating given |
| 'review'   | Review text |


## Data Cleaning and Exploratory Data Analysis
We will first merge the two datasets into one and clean the dataset for further analysis.
### Cleaning



We will first merge the two datasets based on recipe ID such that the new dataset will contain recipes the original recipe dataset
as well as columns that have data about the ratings. In all cases of 0 ratings, we replace it with `np.nan` so the values get ignored instead of dragging the average score down and malforming the data. Then we will aggregate it such that each recipe will have the average rating of the recipe. We then dropped all the individual ratings as well as duplicate user ID's to form a dataset that had one row of unique recipe with an average rating value. 

## Framing a Prediction Problem
Our model will try predict the rating of a recipe using linear regression with values from nutrition, number of ingredients, and number of steps. We wish to predict rating since it is the value that recipes are evaluated by people who use them, offering the best insight as to the value of a recipe. 

The metric we will use to evaluate the model are mean squared errors and R^2 given that the MSE offers us clues as to how big the error is and R^2 will give us clues as to how unfit the model is to the data. 
## Baseline Model

## Final Model