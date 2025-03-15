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

I merged the two datasets using a left join, ensuring that all recipes remained in the dataset, even if they had no ratings. In the merged dataset, I replaced all instances of 0 with NaN as Food.com allows ratings from 1 to 5 and 0 likely represented missing reviews rather than actual ratings. To obtain a new feature capturing overall ratings, I grouped the dataset by 'id' and computed the average rating per recipe, creating the 'average_rating' column. Additionally, I formatted the 'user_id' and 'rating' columns as lists, where each recipe now contains all users who rated it and the corresponding list of ratings.

To focus the dataset on features relevant to predicting 'average_rating' for this project, I removed unnecessary columns and retained only a subset:'id', 'name', 'user_id', 'rating', 'average_rating', 'minutes', 'n_steps', 'n_ingredients','nutrition','description', and 'tags'. The 'nutrition' column originally stored a list of values, which made it difficult to interpret and use in modeling. I expanded this column into separate features: 'calories', 'total_fat', 'sugar', 'sodium', 'protein', 'saturated_fat', and 'carbs', then dropped the original 'nutrition' column.

The 'minutes' column contained extreme and unrealistic outliers so I applied the Interquartile Range method to detect and remove outliers. I defined IQR as Q3 - Q1 and removed any values outside the range [Q1 - 1.5 * IQR, Q3 + 1.5 * IQR], keeping only reasonable cooking times. Finally, I converted 'id' to a categorical column because it represents a unique recipe identifier rather than a numerical feature. During EDA and pplot generation, I created two more columns for 'short_or_long' where short recipes are under 30 minutes and long are 30+ minutes and another for 'low_cal' where low-calorie recipes are categorised as under 500 cals. This plays into the health factor that I plan to further investigate in my predictive model.

After cleaning, the dataset contained 75,791 rows and 19 columns is shown below:


|     id | name                                  | user_id                                                                                                 | rating                                             |   average_rating |   minutes |   n_steps |   n_ingredients | description                                                                                                                                                                                                                      | tags                                                                                                                                                                                                                                                                                                                                    |   calories |   total_fat |   sugar |   sodium |   protein |   saturated_fat |   carbs | low_cal   | short_or_long    |
|-------:|:--------------------------------------|:--------------------------------------------------------------------------------------------------------|:---------------------------------------------------|-----------------:|----------:|----------:|----------------:|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------:|------------:|--------:|---------:|----------:|----------------:|--------:|:----------|:-----------------|
| 275022 | impossible macaroni and cheese pie    | [240552.0, 242794.0, 1221043.0]                                                                         | [4.0, 1.0, 4.0]                                    |                3 |        50 |        11 |               7 | one of my mom's favorite bisquick recipes. this brings back memories!                                                                                                                                                            | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'main-dish', 'eggs-dairy', 'pasta', 'easy', 'cheese', 'dietary', 'high-calcium', 'high-in-something', 'pasta-rice-and-grains', 'elbow-macaroni']                                                                                                     |      386.1 |          34 |       7 |       24 |        41 |              62 |       8 | True      | Short (<30 mins) |
| 275024 | impossible rhubarb pie                | [171303.0]                                                                                              | [3.0]                                              |                3 |        55 |         6 |               8 | a childhood favorite of mine. my mom loved it because it cut down on how much time to make it.                                                                                                                                   | ['60-minutes-or-less', 'time-to-make', 'course', 'preparation', 'healthy', 'pies-and-tarts', 'desserts', 'pies', 'dietary']                                                                                                                                                                                                             |      377.1 |          18 |     208 |       13 |        13 |              30 |      20 | True      | Short (<30 mins) |
| 275026 | impossible seafood pie                | [558429.0, 131804.0]                                                                                    | [1.0, 5.0]                                         |                3 |        45 |         7 |               9 | this is an oldie but a goodie. mom's stand by for company. good enough for us on a special occasion or if company came over!                                                                                                     | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'very-low-carbs', 'main-dish', 'eggs-dairy', 'seafood', 'crab', 'cheese', 'dietary', 'low-sodium', 'low-calorie', 'low-carb', 'low-in-something', 'shellfish']                                                                                       |      326.6 |          30 |      12 |       27 |        37 |              51 |       5 | True      | Short (<30 mins) |
| 275030 | paula deen s caramel apple cheesecake | [156891.0, 276925.0, 723255.0, 55655.0, 437237.0, 951589.0, 739665.0, 115525.0, 231639.0, 2001170768.0] | [5.0, 5.0, 5.0, 5.0, 5.0, 5.0, 5.0, 5.0, 5.0, 5.0] |                5 |        45 |        11 |               9 | thank you paula deen!  hubby just happened to be watching with me one day when she made these and it will always be requested in our home!  it's very easy to make and such a fun twist on a plain cheesecake.  it's a must try! | ['60-minutes-or-less', 'time-to-make', 'course', 'preparation', 'occasion', 'desserts', 'cheesecake', 'gifts', 'taste-mood', 'sweet']                                                                                                                                                                                                   |      577.7 |          53 |     149 |       19 |        14 |              67 |      21 | False     | Short (<30 mins) |
| 275032 | midori poached pears                  | [306797.0]                                                                                              | [5.0]                                              |                5 |        25 |         8 |               9 | the green colour looks fabulous and the taste is heavenly. serve with a raspberry coulis. keep enough rind of the orange and lemon for garnish.                                                                                  | ['lactose', '30-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'cuisine', 'preparation', 'occasion', 'south-west-pacific', 'desserts', 'fruit', 'australian', 'easy', 'beginner-cook', 'dinner-party', 'summer', 'dietary', 'gluten-free', 'seasonal', 'egg-free', 'free-of-something', 'pears', 'taste-mood', 'sweet'] |      386.9 |           0 |     347 |        0 |         1 |               0 |      33 | True      | Short (<30 mins) |

The final cleaned dataset is then used for exploratory data analysis and predictive modeling in the next steps.

I then moved onto univariate analysis. Below is the distribution for the 'average_rating' column:

<iframe
  src="assets/rating_distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The distribution of average ratings is heavily skewed towards 5-star ratings, indicating a strong positivity bias in user feedback. This suggests that users are more likely to rate a recipe when they have a positive experience, which is observed in past research as well. It's due several factors including Acquisition-led selection bias where ratings come from purchasers who are already have favourable attitude towards the recipe, Social influence bias where new raters to be influenced by existing high ratings and Under-reporting bias which states results in extreme experiences (either very positive or negative) are more likely to be reported, often skewing ratings towards positivity.

By analyising, cooking time and average ratings, I found no **strong** correlation as high ratings appear across all cooking durations. However, recipes with shorter cooking times seem to have a higher concentration of 4+ star ratings, suggesting that users may prefer recipes that are quicker and easier to prepare.


<iframe
  src="assets/rating_vs_time.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The pivot table shows the relationship between number of steps, cooking time, and average ratings. Recipes with fewer steps (0-5) tend to have the highest ratings (4.68), especially for very short (0-15 min) and long (61-120 min) cooking times, suggesting that users favor simpler recipes but also appreciate well-executed complex ones.

| n_steps   |    0-15 |   16-30 |   31-60 |   61-120 |
|:----------|--------:|--------:|--------:|---------:|
| 0-5       | 4.68486 | 4.5957  | 4.58659 |  4.61725 |
| 6-10      | 4.65396 | 4.62428 | 4.59882 |  4.62777 |
| 11-20     | 4.6336  | 4.63392 | 4.61208 |  4.62832 |
| 21+       | 4.63171 | 4.6821  | 4.64464 |  4.63185 |


## Assessment of Missingness

I believe the missingness in average_rating may be Not Missing At Random (NMAR). If users choose not to rate a recipe because they had a neutral or negative experience, the missing values depend on the unobserved reason for non-response rather than another recorded variable. This suggests NMAR because the missingness itself is related to the underlying (but uncollected) sentiment toward the recipe which may also be impacted by user bias. To determine if the missingness is instead Missing At Random (MAR), we would need additional data. If missingness can be explained by reaons such as user engagement metrics where a user viewed or saved the recipe but didn’t rate it, it would be MAR.


I conducted  missingness permutation tests to determine if the missing values in 'average_rating' depend on other columns such as 'minutes','n_steps', or 'recipe_id'. The results show extremely low p-values for minutes and n_steps (0.000999000999000999), indicating that missingness in 'average_rating' is not random but depends on these columns. This suggests that longer or more complex recipes might be more likely to have missing ratings. However, the test for recipe_id suggests that missingness is independent of the recipe identifier. 


<iframe
  src="assets/missing_distr.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The embedded empirical distribution plot visualizes the test statistic for missingness in n_steps. The observed test statistic, marked by the red dashed line, falls far outside the bulk of the empirical distribution, reinforcing the conclusion that missingness is not random and is significantly related to n_steps


## Hypothesis Testing

Given that health-conscious consumers may prefer lower-calorie recipes, calorie content could influence ratings. By randomly shuffling the calorie labels and comparing the observed difference to a null distribution, I aim to analyse whether any association is statistically significant.

Null Hypothesis (H₀): The average rating of recipes does not differ significantly between low-calorie and non-low-calorie recipes.

Alternative Hypothesis (H₁): The average rating of recipes differs significantly between low-calorie and non-low-calorie recipes.

Test Statistic: difference in mean average ratings between the two groups.

Significance Level: significance level (α) is set at 0.05.

Observed Difference in Ratings: 0.0053

P-value: 0.325

Since the p-value (0.325) is much greater than the significance level (0.05), we fail to reject the null hypothesis. This suggests that there is *no strong statistical evidence* that average ratings differ between low-calorie and non-low-calorie recipes.  However, this does not rule out the possibility of a non-linear relationship that the test fails to capture.

## Framing a Prediction Problem

Through this project, I aim to predict our **target variable 'average_rating'** for a recipe using a **regression model** based on other features of the recipe to better understand user preferences. At the time of prediction, we would have access to all columns of the cleaned dataframe so we can use the relevant features like cooking time, no. of steps, calorie range, ingredients etc. However, if a recipe does not have any interactions, we would have to limit to only using recipe specific features and exclude factors like no. of ratings for a recipe.

To evaluate the model's performance, we use Root Mean Squared Error (RMSE) and R² (coefficient of determination). RMSE quantifies the average prediction error, penalizing larger deviations, making it useful for assessing how close the predicted ratings are to actual values. A lower RMSE indicates better predictive accuracy. R² measures how well the model explains variance in the ratings, with values closer to 1 indicating a stronger fit. Using both metrics provides a more comprehensive evaluation—RMSE highlights absolute error magnitude, while R² indicates the proportion of variability captured by the model.

## Baseline Model

My basline model is a linear regression that predicts average recipe ratings using three quantitative features: minutes, n_ingredients, and n_steps. Since all features are numerical, no encoding was needed. The numerical features are standardized using StandardScaler, which centers them by subtracting the mean and scales them to unit variance, ensuring comparable scales.

The model’s performance is evaluated using RMSE (Training RMSE: 0.2770, Testing RMSE: 0.2849) and R² (Training R²: 0.2027, Testing R²: 0.1725). While the RMSE is relatively low, the low R² suggests that the model does not explain much of the variation in ratings. This indicates that factors beyond cooking time, ingredients, and steps—such as user preferences or recipe content—likely influence ratings. To improve the model, I could explore non-linear relationships, additional features, or different modeling approaches to increase R².


## Final Model

## Fairness Analysis