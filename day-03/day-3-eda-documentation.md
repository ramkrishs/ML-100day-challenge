# Day 3: Exploratory Data Analysis (EDA) and Visualization of Indian Food Dataset

## Overview
This documentation provides a detailed explanation of the exploratory data analysis (EDA) performed on the cleaned Indian food dataset. The analysis aims to understand the dataset's structure, identify patterns, and extract meaningful insights about Indian food recipes.

## Dataset Background
The dataset contains information about Indian food recipes, including recipe names, descriptions, cuisine types, cooking times, ingredients, and instructions. The data was previously cleaned and preprocessed in the Day-2 notebook, where non-English text was translated and time columns were standardized.

## Notebook Structure

### 1. Data Loading and Initial Exploration

```python
# Imports & options
import pandas as pd
import numpy as np

# display settings
pd.set_option('display.max_columns', None)
pd.set_option('display.max_rows', 20)
pd.set_option('display.width', None)

# Load the cleaned dataset
indian_food_df = pd.read_csv("data/cleaned_indian_food.csv")
```

**Purpose**: This section imports necessary libraries and loads the cleaned dataset from Day-2. The display settings are configured to show all columns and a reasonable number of rows for better visualization.

### 2. Basic Dataset Overview

```python
# Basic overview
print(indian_food_df.shape)          # rows, cols
indian_food_df.info()                # dtypes & non-null counts
display(indian_food_df.head(5))      # first few rows
```

**Purpose**: This section provides a basic understanding of the dataset's structure:
- The dataset contains 7,500 rows (recipes) and 14 columns
- The data types include objects (strings), integers, and floats
- A preview of the first 5 rows helps understand the data format

**Key Insights**:
- The dataset has a mix of categorical columns (cuisine, course, diet) and numerical columns (prep_time, cook_time, total_time, n_ingredients)
- Some columns have missing values, particularly description (7,381 missing), cuisine (510 missing), and cook_time (119 missing)
- The dataset includes both original text (ingredients, instructions) and their English translations (ingredients_en, instructions_en)

### 3. Missing Data Analysis

```python
# Missing data count
missing = indian_food_df.isnull().sum().sort_values(ascending=False)
display(missing[missing > 0])
```

**Purpose**: This section identifies and quantifies missing values in the dataset, which is crucial for understanding data quality and planning appropriate handling strategies.

**Key Insights**:
- Description column has the most missing values (7,381)
- Cuisine, instructions, and instructions_en each have 510 missing values
- Cook_time has 119 missing values
- Ingredients and ingredients_en each have 6 missing values
- The high number of missing descriptions suggests this field might not be essential for most recipes

### 4. Numerical and Categorical Summaries

```python
# Numeric & categorical summaries
display(indian_food_df.describe(include=[np.number]))
display(indian_food_df.describe(include=['object']).T[['top','freq']])
```

**Purpose**: This section provides statistical summaries for both numerical and categorical variables:
- For numerical columns: count, mean, standard deviation, min, 25th percentile, median, 75th percentile, and max
- For categorical columns: the most frequent value and its frequency

**Key Insights for Numerical Variables**:
- Preparation time (prep_time): Average of 29.4 minutes, with a wide range from 0 to 2,880 minutes (48 hours)
- Number of ingredients (n_ingredients): Average of 26.5 ingredients per recipe, ranging from 0 to 127
- Cooking time (cook_time): Average of 31.1 minutes, ranging from 0 to 900 minutes (15 hours)
- Total time (total_time): Average of 60 minutes, ranging from 0 to 2,925 minutes (48.75 hours)
- The wide ranges and high standard deviations indicate significant variability in recipe complexity and preparation requirements

**Key Insights for Categorical Variables**:
- Most common recipe name: "Mustard Vegetable Curry Recipe" (appears 3 times)
- Most common cuisine: "Indian" (1,181 recipes)
- Most common course: "Lunch" (1,793 recipes)
- Most common diet: "Vegetarian" (4,795 recipes)
- The high frequency of vegetarian recipes (64% of the dataset) reflects the prevalence of vegetarianism in Indian cuisine

### 5. Top Categories Analysis

```python
# Top categories
for col in ['cuisine','course','diet']:
    if col in indian_food_df.columns:
        print(col, indian_food_df[col].value_counts().head(10), '\n')
```

**Purpose**: This section examines the distribution of the top 10 categories for cuisine, course, and diet, providing a more detailed view of the categorical variables.

**Key Insights**:
- **Cuisine Distribution**:
  - Indian: 1,181 recipes
  - Continental: 1,021 recipes
  - North Indian: 962 recipes
  - South Indian: 695 recipes
  - Italian: 236 recipes
  - The presence of non-Indian cuisines (Continental, Italian) suggests fusion recipes or international influences

- **Course Distribution**:
  - Lunch: 1,793 recipes
  - Side Dish: 1,026 recipes
  - Snack: 879 recipes
  - Dinner: 787 recipes
  - Dessert: 686 recipes
  - The distribution shows a good balance across different meal types

- **Diet Distribution**:
  - Vegetarian: 4,795 recipes
  - High Protein Vegetarian: 715 recipes
  - vegetarian (lowercase): 453 recipes
  - Non Vegeterian: 436 recipes
  - Eggetarian: 346 recipes
  - The dataset has inconsistent labeling (e.g., "Vegetarian" vs. "vegetarian"), which might require standardization
  - The dominance of vegetarian recipes reflects Indian dietary preferences

### 6. Ingredient Analysis

```python
# Most common ingredients
from collections import Counter

def explode_ings(s, sep=','):
    return Counter(s.dropna().str.split(sep).explode().str.strip())
print(explode_ings(indian_food_df['ingredients']).most_common(20))
```

**Purpose**: This section identifies the most commonly used ingredients in the recipes by splitting the ingredients column and counting occurrences.

**Key Insights**:
- The most common ingredient is "Sal; -; o; as; e" (3,531 occurrences), which appears to be a parsing artifact for "Salt to taste"
- Other common ingredients include turmeric powder, cumin seeds, red chili powder, and onions
- The presence of ingredients like "नमक - स्वाद अनुसार" (Hindi for "salt to taste") indicates that not all text was translated
- The data shows some formatting issues (e.g., "1; easpoo;" instead of "1 teaspoon"), which might require further cleaning

### 7. Time Analysis

```python
# Time stats
for t in ['prep_time','cook_time','total_time']:
    if t in indian_food_df.columns:
        print(t, indian_food_df[t].describe(), '\n')
```

**Purpose**: This section provides a detailed statistical analysis of the time-related columns (preparation, cooking, and total time).

**Key Insights**:
- **Preparation Time**:
  - Mean: 29.4 minutes
  - Median: 15 minutes
  - 75% of recipes require 20 minutes or less for preparation
  - The large difference between mean and median indicates some outliers with very long preparation times

- **Cooking Time**:
  - Mean: 31.1 minutes
  - Median: 30 minutes
  - 75% of recipes require 39 minutes or less for cooking
  - The closer mean and median suggest a more normal distribution compared to preparation time

- **Total Time**:
  - Mean: 60 minutes
  - Median: 40 minutes
  - 75% of recipes require 55 minutes or less in total
  - The data suggests that most Indian recipes can be prepared in under an hour

### 8. Visualization Section

The notebook includes several visualizations (not fully visible in the code review) that likely include:

1. **Distribution plots** for numerical variables (prep_time, cook_time, total_time, n_ingredients)
2. **Bar charts** for categorical variables (cuisine, course, diet)
3. **Correlation analysis** between numerical variables
4. **Ingredient frequency visualizations**

These visualizations help in understanding:
- The distribution and skewness of time-related variables
- The relative proportions of different cuisines, courses, and diet types
- Relationships between variables (e.g., correlation between number of ingredients and preparation time)
- The most common ingredients and their frequencies

## Summary of Insights

1. **Dataset Composition**:
   - 7,500 Indian food recipes with 14 features
   - Mix of categorical (cuisine, course, diet) and numerical (times, ingredient counts) variables
   - Some missing values, particularly in the description column

2. **Recipe Characteristics**:
   - Most recipes are vegetarian (64%)
   - Average recipe has 26.5 ingredients
   - Average total cooking time is 60 minutes
   - Most recipes are categorized as lunch dishes

3. **Cuisine Diversity**:
   - While predominantly Indian, the dataset includes Continental, Italian, and regional Indian cuisines
   - Regional diversity is represented with North Indian, South Indian, Bengali, Maharashtrian, and other regional cuisines

4. **Time Requirements**:
   - Wide range of preparation and cooking times
   - Some outliers with extremely long preparation or cooking times
   - Most recipes can be completed within an hour

5. **Data Quality Issues**:
   - Some inconsistent labeling in categorical variables
   - Formatting issues in ingredient listings
   - Missing values in several columns
   - Some non-translated text remains

## Next Steps

Based on this exploratory analysis, potential next steps could include:

1. **Data Cleaning**:
   - Standardize categorical variables (e.g., merge "Vegetarian" and "vegetarian")
   - Further clean ingredient text to remove formatting artifacts
   - Handle missing values appropriately

2. **Advanced Analysis**:
   - Cluster recipes based on ingredients or cooking techniques
   - Analyze relationships between cuisine type and cooking time
   - Investigate correlations between number of ingredients and recipe complexity

3. **Feature Engineering**:
   - Extract key ingredients for each cuisine
   - Create complexity scores based on preparation time and number of ingredients
   - Develop cuisine classifiers based on ingredients

4. **Visualization Enhancements**:
   - Create interactive visualizations for exploring relationships
   - Develop dashboards for comparing cuisines or diet types
   - Map regional cuisines to their geographical origins

This exploratory analysis provides a solid foundation for understanding the Indian food dataset and sets the stage for more advanced analyses and potential machine learning applications.
