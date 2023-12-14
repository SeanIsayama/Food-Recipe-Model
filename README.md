# Food-Recipe-Model
**Authors**: Hikaru Isayama, Julia Jung

## Project Overview

In our previous data analysis project on, we investigated the relationship between the number of ingredients (#) and amount of calories from the recipes and reviews posted on the food.com dataset from 2008.

Our exploratory data analysis on this dataset can be found [here](https://seanisayama.github.io/Food-Recipe-Study/).

---

## Framing the Problem

### Problem Identification

In this project, we will build a classification model to predict the amount of calories in a given recipe. Specifically, our model will predict the recipe to be in one of the following categories we created:

0-250 calories for `'snack'`, 250-500 calories for `'breakfast'`, 500-750 calories for `'lunch'`, 750-1000 calories for `'dinner'`, 1000-1250 calories for `'hearty'`, 1250-1500 calories for `'banquet'`, and 1500+ calories for `'feast'` (with the first value inclusive, last value exclusive). 

As a result, this model will be a multiclass classification, with our response variable being `'meal type'`. We chose to seperate the quantitative intervals of calories into ordinal categories, as our previous project revealed that there were many severe outliers and inconsistencies in the dataset that will make it difficult for a quantitiative regression model to predict.

Since false positives and false negatives are around equally as bad to have in this project, we chose to evaluate the our model using the f1-score metric - which combines the precision and recall scores of the model to evaluate its accuracy.

---

## Baseline Model

In our baseline model, we will use a Decision Tree Classifier with the features `'n_ingredients'` and `'n_steps'` to predict the amount of calories. 

Both `'n_ingredients'` and `'n_steps'` are quantitative discrete variables, as there are all stored as integer values in the dataset. We will use these features to predict `'meal type'`, which we constructed to be an ordinal variable. To scale quantitative values to unit variance, we also decided to transform both the `'n_ingredients'` and `'n_steps'` features using sklearn's StandarScaler.

Finally, we created a test-train split, fit our data on a DecisionTreeClassifier, and evaluated the score of our model.

| Score type   | Train score         | Test score          |
|-------------:|:--------------------|:--------------------|
| f1 Score     | 0.47707204709294887 | 0.48320651797187847 |

As we can see above, we noticed an f1 Score of around 0.48 for both the training and test sets. This score is not "good," as it is a relatively low score, meaning there is high bias. However, as the train and test scores seems to be around the same, we can see that there is low variance.

Our baseline model does not generalize well to both the training and test sets.

---

## Final Model

In our final model, we decided to add two new features to our model:

`'minutes'`: we chose to 

`'oh_low_calorie'`: in the original food.com dataset, we discovered that there was a tag named `'low_calorie'` that could be a part of the list of tags in a recipe. Therefore, we decided use one-hot-encoding to represent if this tag was present on the receipe or not in this column. We beleive that this new feature will help predict calories, as this tag directly involves the relative amount of calories there is in the recipe.

| Score type   | Train score         | Test score          |
|-------------:|:--------------------|:--------------------|
| f1 Score     | 0.6883168126776259  | 0.6331954101437529  |

---

## Fairness Analysis

---
