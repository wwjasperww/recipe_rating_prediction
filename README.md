# What makes a recipe highly rated?
Final Project for DSC 80 at UCSD

by Zijie Feng

## Introduction

- My dataset is about food recipes and user ratings of these recipes. The dataset itself has 83782 rows and 13 columns, including ['name', 'id', 'minutes', 'contributor_id', 'submitted', 'tags', 'nutrition', 'n_steps', 'steps', 'description', 'ingredients', 'n_ingredients', 'average_rating_recipe']
- The main question I will try to answer is: what types of recipes tend to have a higher average rating? To answer this question, relevant columns would include 'minutes', 'submitted', 'tags', 'nutrition', 'n_steps', 'ingredients', 'n_ingredients', 'average_rating_recipe'. However, since 'ingredients' and 'tags' have too many unique values in them and would make this study much more complicated, I decided to not include these 2.
- 'minutes' contains the number of minutes needed to complete a recipe; 'submitted' has the date when the recipe was submitted; 'nutrition' has the amount of different nutritions a recipe contains; 'n_steps' contains the number of steps to complete a recipe; 'n_ingredients' has the number of ingredients for a recipe; 'average_rating_recipe' has the average rating for a certain recipe across users.
- With these pieces of information, I should be able to get some insight regarding the characteristics that impact the ratings. With such insights, users are able to have a more objective evaluation of a recipe. When checking other people's ratings on a certain recipe, now we can first use the insights gained from this project to determine a reasonable rating and then compare it with other users' ratings.

## Data Cleaning and Exploratory Data Analysis


## Assessment of Missingness


## Hypothesis Testing


## Framing a Prediction Problem


## Baseline Model


## Final Model


## Fairness Analysis

