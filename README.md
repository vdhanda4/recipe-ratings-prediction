# Predicting Average Ratings for Recipes

## Introduction
This project aims to predict the Average Rating for a recipe based on different features. I used two datasets from Food.com, one containing recipe details and the other user-submitted ratings. Since the original datasets were quite large, this project uses the subset files RAW_recipes.csv (83,782 rows, one per recipe) and RAW_interactions.csv (731,927 rows, one per user interaction with a recipe). 

The following is a description of each column for both datasets:

### Recipes Dataset

| Column         | Description  |
|---------------|-------------|
| 'name'        | Recipe name |
| 'id'          | Recipe ID   |
| 'minutes'     | Minutes to prepare recipe |
| 'contributor_id' | User ID who submitted this recipe |
| 'submitted'   | Date recipe was submitted |
| 'tags'        | Food.com tags for recipe |
| 'nutrition'   | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)] |
| 'n_steps'     | Number of steps in recipe |
| 'steps'       | Text for recipe steps, in order |
| 'ingredients'     | List of all ingredients |
| 'n_ingredients'     | Number of ingredients in recipe |
| 'description' | User-provided description |

### Interactions Dataset

| Column       | Description                |
|-------------|----------------------------|
| 'user_id'   | User ID                     |
| 'recipe_id' | Recipe ID                   |
| 'date'      | Date of interaction         |
| 'rating'    | Rating given                |
| 'review'    | Review text                 |


Food plays a central role in daily life, and online recipe platforms help users discover new dishes. Recipe users as well as food bloggers might be interested in this conlcusion as it gives insight into which factors contribute to average ratings. Food bloggers may use this to optimize their recipes to cater to audience preferences. 

Through this project, I aim to answer the central question: **What types of recipes tend to have higher average ratings?** I plan to explore and experiment the influence of features such as cooking time, calorie count, no. of steps, no. ratings etc. to achieve this in the next steps.



## Data Cleaning and Exploratory Data Analysis

## Assessment of Missingness

## Hypothesis Testing

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis