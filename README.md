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

| name            |     id |   minutes | ...   |   protein |   saturated_fat |   carbohydrates |
|:----------------|-------:|----------:|:------|----------:|----------------:|----------------:|
| 1 brownies in t | 333281 |        40 | ...   |         3 |              19 |               6 |
| 1 in canada cho | 453467 |        45 | ...   |        13 |              51 |              26 |
| 412 broccoli ca | 306168 |        40 | ...   |        22 |              36 |               3 |
| millionaire pou | 286009 |       120 | ...   |        20 |             123 |              39 |
| 2000 meatloaf   | 475785 |        90 | ...   |        29 |              48 |               2 |


### Univariate Analysis

<iframe
  src="assets/2_2_1.html"
  width="600"
  height="300"
  frameborder="0"
></iframe>

- Plot 1: Shows a clearly right-skewed shape with the peak being around n_steps equal to 7, indicating that most recipes should have a number of steps less than 20.

<iframe
  src="assets/2_2_2.html"
  width="600"
  height="300"
  frameborder="0"
></iframe>

- Plot 2: Shows that more than half of the recipes have an average rating of 5, and almost all recipes have an average rating above 3.


### Bivariate Analysis

<iframe
  src="assets/2_3_1.html"
  width="600"
  height="300"
  frameborder="0"
></iframe>

- Plot 1: Although not extremely clear, there does seem to be an upward trend as we go from left to right. More carbohydrates potentially correlates with more total fat.

<iframe
  src="assets/2_3_2.html"
  width="600"
  height="300"
  frameborder="0"
></iframe>

- Plot 2: A clear upward movement can be seen as we go from left to right. Sugar seems to have a positive correlation with the amount of calories contained in a recipe.


### Interesting Aggregates

| total_fat   |   Really Low |      Low |   Medium |     High |   Really High |
| minutes     |              |          |          |          |               |
|:------------|-------------:|---------:|---------:|---------:|--------------:|
| Really Low  |      5.66682 |  6.71578 |  7.0395  |  7.34443 |       7.18963 |
| Low         |      7.86316 |  8.80775 |  9.02783 |  9.35648 |       9.48525 |
| Medium      |      8.68417 |  9.84759 |  9.98878 | 10.2119  |      10.5632  |
| High        |      9.03142 | 10.2532  | 10.3551  | 10.6438  |      11.1271  |
| Really High |      8.81313 | 10.3634  | 10.8512  | 11.1486  |      11.7494  |

- When going from top to bottom, the number of ingredients go up since the number of minutes is going from really low to really high, meaning we are probably getting more ingredients involved while spending more time to cook. When going from left to right, the general trend is that with more and more total fat, we are probably adding more ingredients, however, there is one expection in this trend which is the last element in the first row. When the cooking time is really low, the number of ingredients used actually drops when total fat reaches the highest level. Maybe when we are in a hurry, somehow we would prefer to use fewer ingredients that contain more fat so we can consume enough energy efficiently.


## Assessment of Missingness

### NMAR Analysis

- 3 Columns containing missing values: name, description, average_rating_recipe

- The only column that can reasonably be NMAR would be average_rating_recipe. This column was obtained by calculating the group mean ratings of each recipe. If this average rating is missing, then that means it did not receive any rating from the users. One possible reason for why users did not leave a rating would be that they would give a rating so low that they don't feel like actually doing it. Or they want to give a rating of 0 but it wasn't possible to be done. Thus, in this case, the missingness of the average ratings values depend on the ratings themselves, which aligns with the definition of NMAR. If we can obtain some additional information regarding the popularity of each recipe maybe the missingness of the average rating values can be MAR. If a recipe's popularity is very low or close to none, then it's likely that it has not received a rating simply because nobody has tried it out yet. In this case, a recipe's popularity can potentially indicate the missingness of the average rating values, thus making it MAR.

### Missing Dependency

<iframe
  src="assets/3_2_1.html"
  width="600"
  height="300"
  frameborder="0"
></iframe>
<iframe
  src="assets/3_2_2.html"
  width="600"
  height="300"
  frameborder="0"
></iframe>

- Based on the calculated p-value of 3.813369e-13 and the graphs, we can see that the column 'average_rating_recipe' does depend on 'n_steps'. The distribution of 'n_steps' that are missing the average ratings have a mean slightly shifted to the right and a thicker tail, while both distributions are right-skewed. To think about this finding intuitively, maybe it is the case that, with more steps included in the recipe, the recipe will be harder and less likely be tried out by a lot of people. So, maybe not enough people cared enough to give certain recipes a rating, thus the missingness. This insight also helps us think about the main question: what types of recipes tend to get higher average ratings? Maybe a shorter and more concise recipe would be more recommended?

<iframe
  src="assets/3_2_3.html"
  width="600"
  height="300"
  frameborder="0"
></iframe>
<iframe
  src="assets/3_2_4.html"
  width="600"
  height="300"
  frameborder="0"
></iframe>

- On the other hand, with a p-value of 0.112321477466234, we can see from the graphs that the column 'average_rating_recipe' does not depend on 'n_ingredients'. The distribution of 'n_ingredients' with missing ratings is similar to that of the 'n_ingredients' without missing ratings. 


## Hypothesis Testing


## Framing a Prediction Problem


## Baseline Model


## Final Model


## Fairness Analysis

