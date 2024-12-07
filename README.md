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
- We're interested in understanding people's decision-making process when it comes to rating recipes. 
- Specifically, we believe this dataset allows us to explore how individuals rate recipes based on their attributes and characteristics, including the complexity of recipes, the ingredients used, and other relevant factors. 

## Objective
- we aim to identify key attributes that make recipes healthier and more popular. 
- Our findings could then help food bloggers and restaurants develop recipes that align with these characteristics, ultimately making their new offering healthier and more popular.

## Tools and Techniques
- **Python** with **Pandas** for data analysis.
- **Data visualization** to identify trends and correlations.
- **Statistical analysis** to interpret user ratings.

## Key Findings (placeholder)
- Trends in recipe types that tend to have higher ratings.
- Relationships between ingredients or cooking techniques and user satisfaction.
