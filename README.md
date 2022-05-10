# Heart Stroke Prediction Data Science Project: Project Overview
• Produced a logistic regression and decision tree model with an 85% accuracy rate that predicted whether a patient was going to experience a stroke based on multiple variables

• Conducted K-Means clustering models with an NbClust function to determine the optimal number of distinct groups within the data set

• Determined that age, BMI, average glucose levels, and history of hypertension were the most influential variables when predicting a stroke

## Code Used
R

## Results
Concluded that from the K-Means clustering model, most victims of stroke were a part of one of two clusters:
- Cluster 1: consisted of Older Adults with Diabetes and are medically obese, averaging the age of 56 years old, an average glucose level of 202, and a BMI of 33. 
- Cluster 2: consisted of Middle-Aged Adults who do not have diabetes but are medically overweight, averaging the age of 40 years old, an average glucose level of 89.5, and a BMI of 28. 

Concluded from the decision tree that there were only 2 variable coefficients that were seen as significant: age and glucose level were the 2 notable variables to accurately predict the chance of somebody suffering from a stroke.


## Cluster Analysis using KMeans
![](https://github.com/sevesilvestre/StrokePredictionData/blob/main/images/RCluster.png)

## Decision Tree Chart
![](https://github.com/sevesilvestre/StrokePredictionData/blob/main/images/DecisionTree.png)
