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

- **Null Hypothesis**: The distribution of calories of recipes that have an average rating of 5 is the same as the distribution of calories of recipes with average ratings lower than 5.
- **Alternative Hypothesis**: The distribution of calories of recipes that have an average rating of 5 has a higher mean than the distribution of calories of recipes that have average ratings lower than 5.

<iframe
  src="assets/4_1.html"
  width="600"
  height="300"
  frameborder="0"
></iframe>

- The chosen test statistic here is the difference between group means and the significance level is 0.05. One group represents recipes with an average rating of 5 and the other group represents recipes with average ratings lower than 5. After simulating a large amount of mean differences, the observed mean difference lies very far away from the center of the distribution, having a p-value of 0, meaning no simulated test statistic has surpassed the observed one. This leads to the rejection of the null hypothesis in favor of the alternative hypothesis. The result here indicates that it is likely the case recipes with a higher rating tend to have a higher amount of calories. This helps us determine the answer to the main question of this project: what kind of recipes tend to have a higher average rating?

## Framing a Prediction Problem

- The prediction problem will be a regression problem where I will try to build a model to predict values in the 'average_rating_recipe' column. I chose this column because it corresponds to the theme of this project and I belive I can gain more insight about how different factors contribute to the recipe rating, through this prediction problem. The metric I plan on using would be the RMSE as it is the most intuitive way to understand how far off of how accurate our model is performing. In order to build this model, columns including 'minutes', 'n_steps', 'n_ingredients', 'calories' and the 6 columns of nutrition values are all reasonable factors to be considered and they are all information we will be able to know at the time of prediction since the recipe itself could tell us such information.

## Baseline Model

- The baseline model I used was a very simple linear regression model using just 2 columns from the original dataset. The 2 features are 'n_steps', 'calories', which both are quantitative variables. The only column transformer method I used here was the FunctionTransformer, which was used to select out the desired 2 columns from the dataset, for model training. No encodings were really done on my training data since they are both quantitative. I chose these 2 features because there existed evidence in the previous steps of this project that these 2 features might contain valuable information regarding a recipe's rating. After training the baseline model, I was able to obtain an RMSE of about 0.65 which I think can be considered decent performance. If the actual average rating in the future is 4.75 and the model predicted 4.1, I would say this is acceptable.

## Final Model

- The features I added include polynomial feature generated from the 'calories' column with a degree of 2, and the new 'n_steps' column using a binarizer. According to this graph: 

<iframe
  src="assets/7_1.html"
  width="600"
  height="300"
  frameborder="0"
></iframe>

- It seems like the relationship between calories and ratings could be quadratic and so creating polynomial features of degree 2 can help us more easily separate different nodes in the decision tree. Regarding the 'n_steps' column:

<iframe
  src="assets/7_2.html"
  width="600"
  height="300"
  frameborder="0"
></iframe>

- I would like to simplify this numerical column to a binary categorical column because this way, when people are making recipes, they can be more conscious about the ratings they might get when considering whether to exceed this n_steps limit. Both of these 2 features are selected for use in the baseline model, which already produced a pretty good result, so I wanted to improve them further to see if there could be bigger enhancements.

- The modeling algorithm I chose was a Random Forest Regressor, which divides my training data into different groups using layers of decision rules. A Random Forest involves many different decision trees and it makes a decision based on the votes of all of its decision trees, which helps reduce variance significantly. The hyperparameters I chose to tune include n_estimators, max_depth, and min_samples_split. These hyperparameters helped me find a balance between bias and variance. The final selected combination of these hyperparameters is: n_estimators=300, max_depth=15, and min_samples_split=1500. A GridSearchCV was used to find this best combination, while setting the parameter scoring='neg_root_mean_squared_error'. Thus, this combination of hyperparameters provided the best RMSE in my process. While the improvement in RMSE from the baseline model to the final model is minimal, 0.650 - 0.648, I believe tha final model is a better model because it's more sophisticated, using more features, and more robust towards unseen data, quality of ensemble models. If they were to be put in production for real-world situations, the final model should be able to handle better.

## Fairness Analysis

- **Null Hypothesis**: The final model's prediction RMSE is the same for both recipes with lots of steps (n_steps > 7) and recipes with fewer steps (n_steps <= 7).
- **Alternative Hypothesis**: The final model's prediction RMSE is higher for recipes with lots of steps (n_steps > 7) than for recipes with fewer steps (n_steps <= 7).

<iframe
  src="assets/8_1.html"
  width="600"
  height="300"
  frameborder="0"
></iframe>

- Group 1 contains recipes that have n_steps larger than 7 and Group 2 has recipes that have n_steps less than or equal to 7. The evaluation metric we will be using here is the prediction RMSE. The test statistic used for the permutation test is the difference between the prediction RMSE's from both groups (Lots of Steps - Fewer Steps). The significance level used here is the typical value of 0.05. However, the resulting p-value (0.006) is much lower than that, indicating a strong evidence in favor of the Alternative Hypothesis. Thus, we reject the Null Hypothesis. We can conclude that the final model does not achieve RMSE fairness for group 1 and group 2.