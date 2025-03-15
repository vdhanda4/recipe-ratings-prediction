# Predicting Average Ratings for Recipes

## Introduction

This project aims to predict the Average Rating for a recipe based on different features. I used two datasets to perform my analysis, one containing recipe details and the other ratings submitted for the recipes taken from Food.com. Since the original datsets were quite large, this project uses subsets RAW_recipes.csv (83782 rows, one for each recipe) and RAW_interactions.csv (731927 rows, one for each interaction for recipe_id). The following is a description of each column for both datasets: 
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


 decided to conduct our data analysis on checking if we can predict the rating of recipes based on other columns of the dataset. People might be interested in our conclusion because they can figure out what recipes people seemed to approve of based on other columns of the dataset such as the number of steps or how long the recipe is predicted to take. Someone might be interested in the dataset because they enjoy cooking and want to know what recipes people seem to like and why.



## Data Cleaning and Exploratory Data Analysis

## Assessment of Missingness

## Hypothesis Testing

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis
