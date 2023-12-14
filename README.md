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

As a result, this model will be a multiclass classification, with our response variable being `'meal type'`. We chose to seperate the quantitative intervals of calories into ordinal categories, as our previous project revealed that there were many severe outliers and inconsistencies in the dataset that will make it difficult for a quantitiative regression model to preduct.

Since false positives and false negatives are around equally as bad to have in this project, we chose to evaluate the our model using the f1-score metric - which combines the precision and recall scores of the model to evaluate its accuracy.

---

## Baseline Model

In our baseline model, we will use a Decision Tree Classifier with the features `'n_ingredients'` and `'n_steps'` to predict the amount of calories. 

Both `'n_ingredients'` and `'n_steps'` are quantitative discrete variables, as there are all stored as integer values in the dataset. We will use these features to predict `'meal type'`, which we constructed to be an ordinal variable. 



| Score type   | Train score         | Test score          |
|-------------:|:--------------------|:--------------------|
| f1 Score     | 0.47707204709294887 | 0.48320651797187847 |



---

## Final Model



---

## Fairness Analysis

---
