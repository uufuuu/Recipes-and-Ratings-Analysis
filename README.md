# The Secret Sauce of Five-Star Recipes
**Authors:** Becca Fu (beccafuu@umich.edu), Yuchen (Wayne) Yang (siriusyc@umich.edu)

---

## Introduction
### General Overview

The "Recipes and Ratings" dataset provides a comprehensive collection of information about recipes and user interactions, based on data collected from [food.com](https://www.food.com). It consists of two primary CSV files:

1. **Recipes Dataset**: Contains metadata about recipes, including details such as recipe names, preparation times, tags, nutritional content, and user-contributed descriptions.
2. **Interactions Dataset**: Captures user interactions, including ratings and reviews, linking users to the recipes they reviewed.

### Research Question

**How can the average rating of a recipe be determined by its key attributes?**

Understanding this relationship has practical implications for food bloggers, culinary professionals, and recipe-sharing platforms. For instance, analyzing factors like preparation time and nutritional balance can guide recipe creation and marketing strategies.

### Dataset Summary
#### Recipes Dataset
- **Rows:** 83,782
- **Columns:** 12
- Key columns:
  - `minutes`: Preparation time for the recipe.
  - `nutrition`: Nutritional information in the format `[calories, total fat (%DV), sugar (%DV), sodium (%DV), protein (%DV), saturated fat (%DV), carbohydrates (%DV)]`.
  - `n_steps`: Number of preparation steps.

#### Interactions Dataset
- **Rows:** 731,927
- **Columns:** 3
- Key columns:
  - `rating`: User ratings (e.g., 1 to 5 stars).

---

## Data Cleaning and Exploratory Data Analysis

### Part I: Data Cleaning

1. **Merging Datasets**:
   - Left merge `recipes` and `interactions` to retain all recipes, even those without ratings.

2. **Handling Missing Ratings**:
   - Treat `0-star` ratings as missing values.
   - Use imputation strategies:
     - **Listwise Deletion**: Remove recipes with no ratings.
     - **Probabilistic Imputation**: Fill missing ratings by sampling from existing ratings for the same recipe.

3. **Nutritional Breakdown**:
   - Extract details from the `nutrition` column and create separate columns for relevant attributes like `calories` and `protein`.

4. **Categorical Time Variable**:
   - Create `time_involved` with categories:
     - `< 30 minutes`
     - `4 hours or less`
     - `1 day or more`

5. **Focus on Key Columns**:
   - Retain only relevant features such as `average_rating`, `calories`, `minutes`, `n_steps`, and nutritional attributes.

---

### Part II: Univariate Analysis

#### 1. **Difficulty Level**
- Metrics:
  - `minutes`: Preparation time.
  - `n_steps`: Number of steps.

##### Findings:
- Most recipes:
  - Take less than 65 minutes to prepare.
  - Involve fewer than 14 steps.
- Conclusion: Recipes are generally simple and accessible.

#### 2. **Calories**
- Majority of recipes have fewer than 1,000 calories.
- Skewed left distribution indicates a preference for lower-calorie recipes.

---

### Part III: Bivariate Analysis

#### Key Questions:
1. Do simpler recipes receive higher ratings?
2. Are lower-calorie recipes rated higher?

#### Findings:
- **Complexity**:
  - Recipes with fewer than 6 steps or more than 14 steps tend to have higher ratings.
  - Preparation time shows a weak correlation with ratings.

- **Calories**:
  - Low-calorie recipes slightly outperform medium- and high-calorie recipes in ratings.

- **Protein**:
  - Light and moderate protein recipes receive higher average ratings.

---

### Part IV: Interesting Aggregates

#### Highlights:
1. Recipes taking `< 30 minutes`:
   - Highest average rating: 4.64.
   - Lowest average calories and protein levels.
2. Recipes taking `1 day or more` have the highest average calories.

#### Conclusion:
- Preference for convenience and simplicity.
- Trade-off between nutrition and time.

---

## Prediction Framework

### Key Attributes for Prediction
- Complexity:
  - `minutes`
  - `n_steps`
- Energy Level:
  - `calories`
  - `protein`

### Baseline Model: Ridge Regression
- **R²**: `-2.074e-05`
  - Indicates poor performance.

### Final Model Improvements
- Introduced categorical variables:
  - `time_involved`
  - `calorie_level`
  - `protein_level`
- **Reduction in Mean Squared Error (MSE)**: 0.14% improvement.

---

## Conclusion
The analysis reveals that simplicity, lower calorie content, and moderate protein levels positively impact recipe ratings. Future work could explore advanced models to capture nuanced patterns and interactions between variables.
