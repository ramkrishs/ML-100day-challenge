# Day 1: Project Kickoff & Dataset Exploration

**Date:** June 29, 2025

## Overview

Today marked the beginning of the 100-Day ML Challenge. The focus was on setting up the project structure and performing an initial exploration of several datasets related to Indian food recipes.

## Key Activities

1.  **Dataset Collection**: Identified and downloaded four distinct datasets containing Indian food recipes from Kaggle.
2.  **Initial Exploration**: Used a custom `quick_view` function in a Jupyter Notebook (`day-1-explore-indian-food-dataset.ipynb`) to get a high-level overview of each dataset, including its shape, data types, and missing values.
3.  **Data Cleaning and Preprocessing**:
    *   Developed reusable functions to clean and standardize the datasets.
    *   Dropped irrelevant columns (e.g., `image_url`, `flavor_profile`).
    *   Standardized the `ingredients` column by converting comma-separated lists into a consistent semicolon-separated format.
    *   Handled missing values by dropping rows where key information (like `course`) was absent.
4.  **Feature Engineering**:
    *   Created a new feature, `n_ingredients`, to count the number of ingredients in each recipe. This provides a simple measure of recipe complexity.
5.  **Code Refactoring**: Consolidated repetitive cleaning steps into modular, well-documented functions to improve the notebook's readability and maintainability.

## Datasets Used

*   `cuisines.csv`
*   `ifood_new.csv`
*   `indian_food.csv`
*   `indianFoodDatasetCSV.csv`

## Next Steps

The cleaned datasets are now ready for more in-depth Exploratory Data Analysis (EDA) and for potential merging into a single, comprehensive dataset.