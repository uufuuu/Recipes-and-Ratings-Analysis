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
- We decided to use listwise imputation and probablistic imputation, `Imputation` section for justification.
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

<iframe
  src="assets/recipes_styled.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

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

##### 1) Preparation Time
we start with summary statistics, and note this a good way to distinguish between recipe's preparation time:
 - recipes taking less than 20 minutes to prepare is in first quartile
 - recipes taking 20 to 35  minutes to prepare is in second quartile
 - recipes taking 35 to 65 minutes to prepare is in third quartile
 - receipes taking longer than 65 minutes to prepare is in the forth quartile

<iframe
  src="assets/recipe_prep_time.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
- Findings:
  - The graph above shows that the majority of recipes take less than 65 minutes.

##### 2) Number of Steps
we continue with analyzing the distribution of steps taken.
 - recipes taking within 6 steps to prepare is in first quartile
 - recipes taking 7-9 steps to prepare is in second quartile
 - recipes taking 11-13 steps to prepare is in third quartile
 - receipes taking more than 14 steps to prepare is in the forth quartile

<iframe
  src="assets/num_of_steps.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
<iframe
  src="assets/recipe_hist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
- Findings:
  - Combining two graphs above, we see that most recipes are not much complex, and involves fewer than 14 steps, with very few exceeding 20 steps.

- Conclusion:
  - These patterns indicate that recipes are generally simple and accessible, suggesting that they are not overly time-consuming and complex in terms of number of steps.
 
#### 2. Energy Provided: Calories
1. We're also interested in calories of recipes. Specifically, we wonder if
   - there are more recipes associated with high calories, or
   - most recipes tend to have low calories.

<iframe
  src="assets/hist_calories.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
- Findings:
  - The histogram for recipe calories shows that the majority of recipes have calorie counts below 1000, with a steep decline in frequency beyond this point.

- Conclusion:
  -  It appears that most recipes tend to have lower calories. The distribution is heavily skewed to the left, with the highest frequency of recipes having calorie counts below 1000.
  -   This suggests that the majority of recipes are designed to be moderate in calorie content rather than high-calorie content.

### Part III: Bivariate Analysis
- Now we want to look at the statistics of pairs of columns to identify possible associations.
- Specifically, we want to further explore what kind of recipes tend to have higher ratings,and we will investigate this question from two perspective similar to previous analysis:
  - difficulty level
  - calorie level
- There are two questions to answer:
  - Do people tend to rate simple recipe higher?
  - Do people rate recipes with lower calorie level higher?
- To answer these questions, we want to create three scatterplot respectively.
  - Average_Rating vs. Preparation Time
  - Average_Rating vs. n_steps
  - Average_Rating vs. calorie

#### Complexity

<iframe
  src="assets/num_steps_vs_ave_rate.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
- The scatterplot shows that the number of steps does not have a strong visible correlation with the average rating.
- However, if we take a closer look at how ratings differ by different step ranges, we observe that recipes with 6 steps or fewer or more than 14 steps tend to have higher average ratings

<iframe
  src="assets/ave_rate_by_steps.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
<iframe
  src="assets/prep_time_vs_ave_rate.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
<iframe
  src="assets/ave_rate_by_time_inv.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
- Similarly, the scatterplot shows that preparation time does not have a strong visible correlation with the average rating.
- However, if we take a closer look at how ratings differ by different step ranges, we observe that
  - receips with less preparation time tend to have higher average ratings

#### Energy Level

<iframe
  src="assets/ave_rate_by_cal.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
- Based on graph, we observe that low-calorie recipes tend to receive slightly higher average ratings compared to medium- and high-calorie recipes. This implies a preference for lower-calorie recipes, possibly due to perceived health benefits.

#### Recipe Ingredients

- Upon our investigation of original dataset, we noticed one of common tags is `low-protein`.
- Therefore, we want to see if average rating differs by its protein(PDV)

<iframe
  src="assets/protein_vs_ave_rate.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
- The scatterplot shows that protein(PDV) does not have a strong visible correlation with the average rating.
- However, if we take a closer look at how ratings differ by ranges of protein(PDV), we observe that
  - receips with light protein level and moderate protein level tend to receive higher average ratings
 
<iframe
  src="assets/ave_rate_by_cal_box.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
### Part IV:Interesting Aggregates
<iframe
  src="assets/aggregation_styled.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Findings:
1. recipes that take "less than 30 minutes"
   - have the highest average rating of 4.64.
   - have the lowest
     - average calories
     - average protein (PDV)
     - average sugar(PDV)
2. receips that takes "1 day or more" have the highest average calories

Conclusion:
1. Preference for Convenience:
   - The high rating for short-preparation recipes suggests that many people value simplicity and speed.
   - This is particularly relevant for recipe developers and food bloggers focusing on gaining popularity; offering easy and quick recipes could attract more users.

2. Trade-off Between Nutrition and Time:
    - Recipes that require more time tend to have richer nutritional content. This suggests a trade-off where users must balance convenience with the desire for more nutrient-dense meals.
    - Consumers who have more time to cook tend to opt for more complex and nutritionally dense dishes

### Part V:Imputation
- In all relevant columns of our analysis, only the rating contains missing value. 
- We decided to choose conditional probabilistic imputation and listwise deletion to deal with missing values. The following are justifications:
  - Listwise Deletion:
    - No useful information provided
      - Recipes without any ratings do not provide any feedback about their quality or popularity. Since our analysis is focused on identifying the attributes that contribute to higher ratings, recipes with no ratings have no value in informing this relationship. They are, therefore, not useful in helping us understand the impact of recipe attributes on ratings.
    - Avoid Bias
      - Recipes with no ratings would require imputation based on attributes alone, which risks introducing fabricated or biased information. Since there are no user evaluations to guide this imputation, any values we add would essentially be guesses, reducing the reliability of our analysis.
  - Conditional Probabilistic Imputation
    - Minimizing Bias: 
      - By drawing from the existing ratings of the same recipe, this approach minimizes the risk of introducing bias that could come from using values unrelated to the recipe itself.
And we analyzed the drawbacks of other strategies as follows
  - (Conditional) mean imputation:
     - Filling missing values with the mean of observed values can reduce variability and introduce bias. 
  - Regression imputation.
     - As we are predicting ratings using other features, doing so will produce imputed values that are highly correlated with the predictors, which artificially inflates any relationships detected in subsequent analysis.

## Framing a Prediction Problem
- Based on above exploratory data analysis, we see that average rating of a recipes differs by
  - complexity
  - energy level
  - protein level 
- Moving forward, we will predict ratings of recipes using its key attributes, including
  - preparation time: measured by `minutes`
  - number of steps: measured by `n_steps`
  - calories measured by `calories`
  - protein level: measured by `protein(PDV)`
 
## Baseline Model
- Model Selection: Ridge Regression
- Justification:
  - As we includes both `minutes` and `n_steps` as our predictors, we think there might exist multicollinearity due to correlation between them.
    - On the one hand, more number of steps might indicate longer preparation time.
    - On the other hand, we think time might still contribute some additional infomation regarding the complexity of receipes.
      - it could be possible that most steps are simple, so the total preparation time is shorted than one might expected simply based on the number of steps.
- As average rating of recipes differs by complexity, we will first predict rating based on two features: `minutes` and `n_steps`

### 1. Model Performance
- **R² (Coefficient of Determination)**: `-2.074e-05`
  - R² explains the proportion of variance in the target variable that can be explained by the predictors. A value close to 1 is ideal, while negative values indicate that the model performs worse than predicting the mean of the target variable.

### Analysis
- **R²**:
   - An R² value of `-2.074e-05` indicates that the model performs no better than the baseline model (predicting the mean of `average_rating`), which is a strong sign that the predictors (`minutes` and `n_steps`) are not sufficiently explaining the variability in the target variable.

### Evaluation
- **No**, the current model is not good:
  - **Low R²**: The very low (and negative) R² indicates that the model has no explanatory power.

## Final Model
### Discussion
- In bivariate analysis, we see that average rating differs by different levels of
  - preparation time,
  - calories
  - protein(PDV)
- To capture these patterns, we want to instead use three categorical variables that we previously defined
  - `time_involved`
  - `calorie_level`
  - `protein_level` 
#### Step 1. Includes new features `time_intervals``calorie_level`, and `protein_level`
#### Step 2. Encoded them 
#### Step 3. Scaling
- Once we encoded all three categorical variables, they will all range from 0 to 1.
- As shown above, `n_steps` ranges from 1 to 20
- Therefore, we normalize all predictors before regression
#### Step 4. search for the best hyperparameters $\lambda$ of ridge regression 
#### Step 5. regression process
#### Step 6. Model Performance Comparison: Final Model vs. Baseline Model

- Baseline Model Mean Squared Error:0.41092464835782266
- Final Model Performance Mean Squared Error: 0.4103397379674629

#### Improvement in Performance
1. **Reduction in MSE:**
  \[
\text{Improvement} = 0.41092464835782266 - 0.4103397379674629 = 0.0005849103903597768
   \]

- The final model has reduced the average squared error by approximately **0.0006**.

2. **Percentage Improvement:**
  \[
\text{Percentage Improvement} = \frac{0.0005849103903597768}{0.41092464835782266} \times 100 \approx 0.14\%
\]
- This indicates that the final model has improved the baseline performance by around **0.14%**.

