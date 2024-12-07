# Recipes and Ratings Analysis
Authors: Becca Fu(beccafuu@umich.edu), Yuchen(Wayne) Yang(siriusyc@umich.edu)

## Introduction
### General Introduction

- The "Recipes and Ratings" dataset provides a comprehensive collection of information about recipes and user interactions with those recipes, based on the data collected from [food.com](https://www.food.com).

- It is split into two main CSV files.

- The "recipes" data set contained metadata about recipes, providing a complete snapshot pf the recipes and their attributes, making it essential for understanding what each recipe offers. It includes information like recipes names, preparation times, tags, nutritional content, number of steps, and user-contributed descriptions.

- The "interactions" data set captures user interactions with recipes, including ratings and reviews. The data links users to the recipes they reviewed, allowing for an analysis of trends in user feedback and recipe popularity.

The central question we are interested in is: **How average rating could be deterimined by key attributes of a recipe?**

- Understanding how average ratings are determined by recipe attributes has practical applications. By analyzing this data, food bloggers, culinary professionals, and recipe-sharing platforms can tailor their recipes to maximize user satisfaction. For example, a recipe with a shorter preparation time and balanced nutritional value might perform better, which can guide recipe creation and marketing strategies.


### Dataset Details Introduction
#### Recipes
- There exist 83782 rows and 12 columns in the recipes file.
- Here are the relevant columns and their descriptions:
'minutes': Time required to prepare the recipe.
'nutrition': Nutritional information in the format calories, total fat (%DV), sugar (%DV), sodium (%DV), protein (%DV), saturated fat (%DV), carbohydrates (%DV). For this column, we focus on calories and protein specifically.
'n_steps': Number of steps required to prepare the recipe.
- Here are the other columns but not so relevant to our research goal:
'name': Name of the recipe.
'id': Unique identifier for each recipe.
'tags': Keywords describing the recipe, such as cuisine type or dietary restrictions.
'description': User-provided description of the recipe.

#### Interactions
- There exist 83872 rows and 731927 columns in the interactions file.
- Here are the relevant columns and their descriptions:
'rating': Numerical score given by the user to rate the recipe (e.g., 1 to 5 stars).
- Here are the other columns but not so relevant to our research goal:
'user_id': Unique identifier for the user providing the rating.
'recipe_id': Identifier linking the rating to a specific recipe.


## Data Cleaning and Exploratory Data Analysis
### Part I: Data Cleaning
1. We start with two individual datasets, one for recipes, the other for ratings. To proceed, we need to left merge the recipes and interactions datasets together.
   - We choose left merge because we want to ensure that all the recipes are retained in the merged dataset, even if some recipes have no ratings or interactions.

2. Once we get the combined dataset, we need to deal with 0-star ratings.
   - In this case, we choose to treat all ratings of 0 as missing values first.
   - 0 is likely indicate that no ratings is given, so it currently doesn't provide meaningful information about user preferences.
     
3. Before proceeding to any column operations, we want to first deal with missing values:
   - we start by examing which columns contains missing values.
   - then we fill missing values in columns relevant to our analysis, including
     - `nutritions`
     - `rating`
     - `minutes`
     - `n_steps`

4. Among all columns relevant to our anlysis, only `rating` contains missing value.
- We decided to use listwise imputation and probablistic imputation, see $\textbf{Imputation}$ section for justification.
  - listwise imputation: we drop all receipe that does not receive any rating.
  - probalistic imputation, we fill missing value in `rating` by drawing random sample from all ratings received by that receipe.

5. We will measure a recipe's popularity by its average rating, so we created a new column that record average rating per recipe.
   
6. Previously, we notice that calories are stored in the `nutrition` column.
   - `nutrition` column contains values that looks like lisst, but they are actually strings.
     - `[calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]`
   - so we need to extract these information from the list-like string.
   - after that, we create columns that record them per receipe.
  
7. We define a new categorical column named 'Time Involved'
   - we categorize recipes into three different categories based on their preparation time
     - `less than 30 minutes`
     - `4 hours or less`
     - `1-day-or-more`

8. from now on, we will only focus on relevant columns, including
   -`averge_rating`
   - `minutes`
   - `time involved`
   - `calories`
   - `n_steps`
   - `total fat(PDV)`
   - `sugar(PDV)`
   - `sodium(PDV)`
   - `protein(PDV)`
   - `saturated fat(PDV)`
   - `carbohydrates(PDV)`

- more explanation on `recipes`
  - each row represent a particular recipe, there are 83782 recipes in total
  - each column except `average_rating` is a feature that we are used to predict `average_rating`

Below is the head of our league_clean dataframe.



### Part II: Univariate Analysis
#### 1. Difficulty Level
1. We are interested in difficulty level of receips. Specifically, we wonder if recipes posted are complex in general.
2. We think two features can reflect the difficulty level, time and n_steps.
   - Intuitively, this makes sense because
     - the more complex a recipe is, the longer time it takes to prepare.
     - the more complex a recipe is, the more steps it involves.
4. To get some insights, we decided to draw two histograms to get some insights.
   - Histograms of Time
   - Histograms of n_steps

#### 1) Preparation Time
we start with summary statistics, and note that
 - recipes taking less than 20 minutes to prepare is in first quartile
 - recipes taking 20 to 35  minutes to prepare is in second quartile
 - recipes taking 35 to 65 minutes to prepare is in third quartile
 - receipes taking longer than 65 minutes to prepare is in the forth quartile


