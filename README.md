# Food-Recipe-Model
**Authors**: Hikaru Isayama, Julia Jung

## Project Overview

In our previous data analysis project on, we investigated the relationship between the number of ingredients (#) and amount of calories from the recipes and reviews posted on the food.com dataset from 2008.

Our exploratory data analysis on this dataset can be found [here](https://seanisayama.github.io/Food-Recipe-Study/).

---

## Framing the Problem

### Problem Identification

In this project, we will build a classification model to predict the amount of calories in a given recipe. Specifically, our model will predict the recipe to be in one of the following categories we created:

|Category|Amount of calories|
|-------------|---------|
|`'snack'`	  |0-250    |
|`'breakfast'`|250-500  |
|`'lunch'`	  |500-750  |
|`'dinner'`	  |750-1000 |
|`'hearty'`	  |1000-1250|
|`'banquet'`  |1250-1500|
|`'feast'`	  |1500+    |

^(with the first value inclusive, last value exclusive)

As a result, this model will be a multiclass classification, with our response variable being `'meal type'`. We chose to seperate the quantitative intervals of calories into ordinal categories, as our previous project revealed that there were many severe outliers and inconsistencies in the dataset that will make it difficult for a quantitiative regression model to predict.

Since false positives and false negatives are around equally as bad to have in this project, we chose to evaluate the our model using the f1-score metric - which combines the precision and recall scores of the model to evaluate its accuracy.

At the time of our prediction, we will have access to all the information (features) since all must be uploaded to food.com when publishinga recipe. However, as we are predicting the amount of calories, we will assume that features such as nutrition values are not available at the time of prediction, but features such as `'n_steps'`, `'n_ingredients'`, `'minutes'`, `'tags'`, and `'rating'` are available.

---

## Baseline Model

In our baseline model, we will use a Decision Tree Classifier with the features `'n_ingredients'` and `'n_steps'` to predict the amount of calories. 

Both `'n_ingredients'` and `'n_steps'` are quantitative discrete variables, as there are all stored as integer values in the dataset. We will use these features to predict `'meal type'`, which we constructed to be an ordinal variable. To scale quantitative values to unit variance, we also decided to transform both the `'n_ingredients'` and `'n_steps'` features using sklearn's StandarScaler.

Finally, we created a test-train split, fit our data on a DecisionTreeClassifier, and evaluated the score of our model.

| Score type   | Train score         | Test score          |
|-------------:|:--------------------|:--------------------|
| f1 Score     | 0.47707204709294887 | 0.48320651797187847 |

As we can see above, we noticed an f1 Score of around 0.48 for both the training and test sets. This score is not "good," as it is a relatively low score, meaning there is high bias. However, as the train and test scores seems to be around the same, we can see that there is low variance.

Therefore, we can say that our baseline model does generalize well to both the training and test sets.

---

## Final Model

In our final model, we decided to add two new features to our model:

`'minutes'`: we chose to use minutes, as we believed that although cooking time itself may not be the determining factor,  the preparation steps involved during that time could contribute to higher caloric content. Additionally, we saw that it was a quantitative variable with not many severe outliers in the dataset.

`'oh_low_calorie'`: in the original food.com dataset, we discovered that there was a tag named `'low_calorie'` that could be a part of the list of tags in a recipe. Therefore, we decided use one-hot-encoding to represent if this tag was present on the receipe or not in this column. We believe that this new feature will help predict calories, as this tag directly involves the relative amount of calories there is in the recipe.

| Score type   | Train score         | Test score          |
|-------------:|:--------------------|:--------------------|
| f1 Score     | 0.6883168126776259  | 0.6331954101437529  |

---

## Fairness Analysis

In this final section, our goal is to answer the question: "Does our final model perform better for recipes with a lower number of ingredients than with a high number of ingredients?" For our investigation, we will use a permutation test, where we shuffle the `'meal type'` response variable on 1000 simulations.

**Null Hypothesis**: Our final model will perform roughly the same between recipes with a lower number of ingredients and with a high number of ingredients, and any differences are likely due to chance (our model is fair)

**Alternative Hypothesis**: Our final model will perform better for recipes with a higher number of ingredients than recipes with a lower number of ingredients. 

**Evaluation Metric**: We will use accuracy as our evaluation metric to determine the performance of our model.

**Test Statistic**: We will use the difference in accuracy scores as our test statistic. We use the difference in means and not the absolute difference since our alternative hypothesis indicates direction.

**Significance Level**: We will use a significance level of 0.01 for our conclusion to ensure accuracy.

The first steps we took to approach this analysis was to use sklearn's Binarizer preprocessing operation to split our data into two groups. More specifically, we created a Binarizer with a thredshold set to the median value of our feature `'n_ingredients'`, to create two groups: recipes with a low number of ingredients, and recipes with a high number of ingredients.

The graph below displays the empirical distribution of our test statistic in 1000 permutations, under the null hypothesis stated above.

<iframe src="assets/hypothesis.html" width=800 height=600 frameBorder=0></iframe> 

From the simulation, we found the p-value to be 0.0, which is less than our significane level of 0.01 for this hypothesis test. Therefore, we can reject the null hypothesis: the results of the test strongly suggest that the observed differences in our samples are not due to random chance, and that our final model tends to perform better with recipes with a higher number of ingredients than recipes with a lower number of ingredients.

---
