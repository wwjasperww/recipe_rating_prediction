# What makes a recipe highly rated?
Final Project for DSC 80 at UCSD

by Zijie Feng

## Introduction

- My dataset is about food recipes and user ratings of these recipes. The dataset itself has 83782 rows and 13 columns, including ['name', 'id', 'minutes', 'contributor_id', 'submitted', 'tags', 'nutrition', 'n_steps', 'steps', 'description', 'ingredients', 'n_ingredients', 'average_rating_recipe']

- The main question I will try to answer is: what types of recipes tend to have a higher average rating? To answer this question, relevant columns would include 'minutes', 'submitted', 'tags', 'nutrition', 'n_steps', 'ingredients', 'n_ingredients', 'average_rating_recipe'. However, since 'ingredients' and 'tags' have too many unique values in them and would make this study much more complicated, I decided to not include these 2.

- 'minutes' contains the number of minutes needed to complete a recipe; 'submitted' has the date when the recipe was submitted; 'nutrition' has the amount of different nutritions a recipe contains; 'n_steps' contains the number of steps to complete a recipe; 'n_ingredients' has the number of ingredients for a recipe; 'average_rating_recipe' has the average rating for a certain recipe across users.

- With these pieces of information, I should be able to get some insight regarding the characteristics that impact the ratings. With such insights, users are able to have a more objective evaluation of a recipe. When checking other people's ratings on a certain recipe, now we can first use the insights gained from this project to determine a reasonable rating and then compare it with other users' ratings.

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

- I merged the recipes and interactions datasets together to pair the user ratings with the recipes themselves. In the merged dataset, I changed ratings of 0 to np.NaN values since these 0's don't actually mean a rating of 0. Instead, they simply mean a rating was not placed. It was only with this change, would the calculation of the mean ratings for each recipe make sense. Then, I attached the column of calculated average ratings for each recipe to the right of the original recipes dataset, with a left merge.

- Another critical step was that I converted the column of 'nutrition' into several columns each representing the amount of a certain element contained by the recipes. The original 'nutrition' column contains strings that are formatted like lists. So I first converted them into real lists, and then converted this series of lists into an array of arrays, which was converted to a dataframe, that is merged back to the recipes dataset. Also, the original 'nutrition' column was dropped.

- Another small fix was the conversion of the 'submitted' column into pd.datetime format.

- There are 3 columns with missing values and one of them is really important, which is the 'average_rating_recipe' column. So, I used probabilistic imputation to fill in the missing values by drawing from other existing values in the column.

- At this point, all the features I am concerned with should be converted to formats that are easy to work with, without any missing values.

- **Head of Cleaned Dataset:**


| name | id | minutes | contributor_id | submitted   | tags | n_steps | steps | description | ingredients | n_ingredients | average_rating_recipe | calories | total_fat | sugar | sodium | protein | saturated_fat | carbohydrates |
|:-----------|-------:|----------:|-----------------:|:------------|:-----------|----------:|:-----------|:--------------|:--------------|----------------:|------------------------:|-----------:|------------:|--------:|---------:|----------:|----------------:|----------------:|
| 1 brownies | 333281 |        40 |           985201 | 2008-10-27  | ['60-minut |        10 | ['heat the | these are     | ['bittersw    |               9 |                       4 |      138.4 |          10 |      50 |        3 |         3 |              19 |               6 |
| 1 in canad | 453467 |        45 |          1848091 | 2011-04-11  | ['60-minut |        12 | ['pre-heat | this is th    | ['white su    |              11 |                       5 |      595.1 |          46 |     211 |       22 |        13 |              51 |              26 |
| 412 brocco | 306168 |        40 |            50969 | 2008-05-30  | ['60-minut |         6 | ['preheat  | since ther    | ['frozen b    |               9 |                       5 |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 |
| millionair | 286009 |       120 |           461724 | 2008-02-12  | ['time-to- |         7 | ['freheat  | why a mill    | ['butter',    |               7 |                       5 |      878.3 |          63 |     326 |       13 |        20 |             123 |              39 |
| 2000 meatl | 475785 |        90 |          2202916 | 2012-03-06  | ['time-to- |        17 | ['pan fry  | ready, set    | ['meatloaf    |              13 |                       5 |      267   |          30 |      12 |       12 |        29 |              48 |               2 |



## Assessment of Missingness


## Hypothesis Testing


## Framing a Prediction Problem


## Baseline Model


## Final Model


## Fairness Analysis

