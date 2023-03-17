<h1 align="center">Heart Stroke Prediction Data Science Project</h1>
<p align="center">
  <img src= "https://github.com/sevesilvestre/StrokePredictionData/blob/main/images/heart.jpeg">
</p>

## Overivew
Motivated by the staggering statistic that 10 million people suffer long-term damage from strokes annually, this project aimed to predict the likelihood of stroke in individuals based on 11 relevant variables. Using advanced analytical techniques such as Logistic Regression, KMeans Clustering, and Decision Tree analysis, our team was able to achieve an 85% accuracy rate in comparing coefficients, and identify distinct clusters to predict the probability of stroke in individuals.

## Code Used
R

## The questions that I aimed to address through my data project are:
- Which variables have the most significant impact on the likelihood of a stroke occuring?
- What are key variables that are most common within stroke victims through a clustering model?
- Can we determine a pattern of variables that lead to a stroke through a decision tree model?

## Key Findings
- Three variables- Age, Hypertension, and Average Glucose Level- had the highest positive relationship with the probability of stroke according to our linear regression model.
- Concluded that from the K-Means clustering model, most victims of stroke were a part of one of two clusters:
  - Cluster 1: consisted of Older Adults with Diabetes and are medically obese, averaging the age of 56 years old, an average glucose level of 202, and a BMI of 33. 
  - Cluster 2: consisted of Middle-Aged Adults who do not have diabetes but are medically overweight, averaging the age of 40 years old, an average glucose level of 89.5, and a BMI of 28. 
- Age and glucose level were the 2 notable variables to accurately predict the chance of somebody suffering from a stroke according to our decision tree model: 

## Data Gathering
With these questions I had, I downloaded a dataset from Kaggle to help me answer these questions: 
- **[Healthcare Stroke Prediction Data Set](https://www.kaggle.com/datasets/fedesoriano/stroke-prediction-dataset)** (Soruce: Kaggle)

## Data Cleaning + Data Manipulation
After loading the dataset into RStudio, the first order of business is drop any columns that will not be necessary for this analysis, as well as altering the data types for variables for optimal use.

```
stroke <- stroke %>% 
  select(-id)
  
stroke$gender <- as.factor(stroke$gender)
stroke$ever_married <- as.factor(stroke$ever_married)
stroke$work_type <- as.factor(stroke$work_type)
stroke$Residence_type <- as.factor(stroke$Residence_type)
stroke$smoking_status <- as.factor(stroke$smoking_status)
stroke$heart_disease <- factor(stroke$heart_disease, levels = c(0,1), labels = c("No", "Yes"))
stroke$hypertension <- factor(stroke$hypertension, levels = c(0,1), labels = c("No", "Yes"))
stroke$bmi <- as.numeric(stroke$bmi)
stroke$stroke <- as.factor(stroke$stroke)

stroke <- na.omit(stroke)
```

## Summary Statistics 
Prior to diving into the model creations, it's important to understand our dataset by analyzing the summary statistics. Using the following code, general information about our dataset is produced which will be helpful when creating our models to predict the probability of a stroke:
```
summary(stroke)
```
This dataset accounts for 4,909 people, identified as Male, Female, or Other, analyzing each of them against 10 other variables showing if they have had a stroke or not. Of this pool of people, 2,987 or 59% were female, 2,011 or 40.9% were male, and just one person identified as other. Most of this dataset is made up of middle-aged adults as the first quartile is at 25 years old, the median at 44 years old, and third quartile at 60 years old. 

Small percentages of the data subjects experienced hypertension and heart disease in their past at 9.2% and 4.95%, respectively. 

Roughly 2⁄3 (65.3% or 3,204) of the test subjects have been married or are married.

The minimum average glucose level among the subjects was 55.12, the mean was 105.31, and maximum was 271.74. It is recommended by health experts to have a glucose level below 140 to be determined healthy. Body mass index or BMI is used to screen for potential health issues. The minimum BMI is 10.30, the median is 28.10, and the maximum is 97.60. Health specialists recommend a BMI between 18.5-24.9 to determine a healthy weight.

As we know smoking is detrimental to one’s health and in this dataset, 837 (17.1%) formerly smoked, 737 (15%) currently smoke, 1,852 (37.7%) have never smoked, and it is unknown the smoking status of 1,483 (30.2%) of our subjects. Lastly, strokes may not seem very common as 4,700 (95.7%) of the test subjects have never had a stroke versus the 209 (4.3%) that have.

## Question 1: Which variables have the most significant impact on the likelihood of a stroke occuring?

To answer this question, we implemented a linear regression model to analyze the relationship between different variables and the probability of a stroke. The following code showcases the creation of the model, consisting of developing the training and testing splits:

```
stroke_split <- initial_split(stroke, prop=0.75)
stroke_train <- training(stroke_split)
stroke_test <- testing(stroke_split)
logit1 <- glm(stroke ~.,
              data = stroke_train,
              family = binomial)
```
<p align="center">
  <img src= "https://github.com/sevesilvestre/StrokePredictionData/blob/main/images/heartcoef.png">
</p>

We discovered that hypertension, or high blood pressure, has the highest correlation to the stroke variable. Following hypertension, the patient’s smoking status had the next highest correlation. Those with high blood pressure and those who smoke had a higher risk of experiencing a stroke. 

We implemented ROC curves to assess the overall diagnostic performance of a test and to compare the performance of two or more diagnostic tests:

<p align="center">
  <img src= "https://github.com/sevesilvestre/StrokePredictionData/blob/main/images/heart%20ROC.png">
</p>

The accuracy scores for both training and testing sets were high at 85%. This indicated that our model was fairly accurate when performing on unseen data to predict the odds of a patient experiencing a stroke.

We also implemented a confusion matrix to evaluate the performance of the classification models, when they make predictions on test data, and tells how good our classification model is. It lets us know when our predictive model made an error if it incorrectly tags someone through false positives or false negatives

<p align="center">
  <img src= "https://github.com/sevesilvestre/StrokePredictionData/blob/main/images/heartconf.png">
</p>

The confusion matrix showcased that our model had more false negatives than false positives. This indicated that there were many cases in which our model falsely predicted that patients would not experience a stroke and this is detrimental because those patients would be missing out on treatment and preventative measures.

Finally to solidify the accuracy of our predictive model, an AUC curve was used since it is a much better indicator of model performance. This is because AUC uses the True Positive Rate and False Positive Rate of the model across different cut-off thresholds.

<h4 align="center">Training Split AUC: </h4>
<p align="center">
  <img src= "https://github.com/sevesilvestre/StrokePredictionData/blob/main/images/hearttrainauc.png">
</p>

<h4 align="center">Testing Split AUC:</h4>
<p align="center">
  <img src= "https://github.com/sevesilvestre/StrokePredictionData/blob/main/images/hearttestauc.png">
</p>

Through our analyses, we discovered that our model was slightly underfit, but our AUC scores were fairly good at 84% and 87% which indicated that our model was better at predicting true positives and true negatives where the patients are correctly predicted in whether or not they will experience a stroke.

## Question 2: - What are key variables that are most common within stroke victims through a clustering model?
Before creating our clustering model, we took a random sample of 1000 observations instead of utilizing all 5000 observations. When creating our clustering model, we implemented only the continuous variables in our data set which were “age”, “average glucose level”, and “bmi”.

When setting up a clustering model, it's necessary to specify how many clusters you want your data to be split up into. This was done through the Elbow method, Silhoutte method, Gap Statistic method, and using the "NbClust" funtion to analyze 24 other methods to determine the the number of clusters: 

<p align="center">
  <img src= "https://github.com/sevesilvestre/StrokePredictionData/blob/main/images/heartcluster.png">
</p>

We determined that the majority of the methods concluded that the optimal number of clusters to be created were 2 clusters. Then, we can create the K-Means clustering model and analyze the results

<p align="center">
  <img src= "https://github.com/sevesilvestre/StrokePredictionData/blob/main/images/RCluster.png">
</p>

<p align="center">
  <img src= "https://github.com/sevesilvestre/StrokePredictionData/blob/main/images/heartcluststats.png">
</p>

From our clustering model, we were able to discover that the data points created 2 clusters representing 78% of point variability. 

Cluster 1 describes people averaging the age of 56 years old, an average glucose level of 202, and a BMI of 33. This cluster represents Older Adults and due to their high glucose levels and high BMI, we can assume that these people have Diabetes and are medically obese. 

Cluster 2 describes people averaging the age of 40 years old, an average glucose level of 89.5, and a BMI of 28. This cluster represents Middle Aged Adults who are technically overweight according to the BMI scale and do not have diabetes. 

Because our model represented 78% of point variability, that would mean that the remaining 22% of data points would be outliers that do not fit into either cluster. The clustering model informed us that people on the unhealthier side are more susceptible to having a stroke with both clusters representing those with an unhealthy lifestyles. 

## Question 3: Can we determine a pattern of variables that lead to a stroke through a decision tree model?

With a decision tree model, we are able to pinpoint different ‘splits’ of decisions or outcomes based on our data set. The decision tree set boundaries as to what different criterias being meant may affect somebody’s chances of having a stroke. 

Because our group was able to see which variables were significant to the linear regression model, we chose to create a decision tree against “age”, “hypertension”, and “average glucose level” since these were the only variables statistically significant.

<p align="center">
  <img src= "https://github.com/sevesilvestre/StrokePredictionData/blob/main/images/heartdecision.png">
</p>

The Decision Tree that we produced stated that if somebody was under the age of 67 then their chances of having a stroke is roughly 2% meanwhile if an individual was over the age of 67 then their chances of having a stroke is roughly 15%. If a patient that is over 67.5 years of age arrives with high glucose levels then it is strongly recommended that medical personnel do screenings in order to isolate symptoms that could result in a stroke.

## Conclusion:
Although most of our test subjects did not suffer from a stroke, those that did showed similar patterns from the other variables. We were able to see that the three variables- Age, Hypertension, and Average Glucose Level- were statistically significant according to our linear regression model. Other variables such as Smoking and BMI, we expected to have a strong effect on predicting having a stroke, but the data showed that they were not statistically significant.

Most victims of stroke were a part of one of two clusters. Cluster 1 consisted of Older Adults with Diabetes and are medically obese, averaging the age of 56 years old, an average glucose level of 202, and a BMI of 33. Cluster 2 consisted of Middle Aged Adults who do not have diabetes but are technically medically overweight, averaging the age of 40 years old, an average glucose level of 89.5, and a BMI of 28.

According to the model that was produced by the decision tree, there were only two coefficients that were seen as significant. It was determined that age and average glucose level were the only two notable variables to accurately predict the chances of somebody suffering from a stroke.

Through our models, we noticed that age, average glucose level, and BMI played larger factors in predicting a stroke and is an area that the healthcare industry must continue to keep a close watch on. By understanding this pattern of high glucose levels and high BMI increasing the probability of a stroke, better initiatives such as programs to assist patients with unhealthy lifestyles at an earlier time to reduce the chance of a stroke could be implemented.

With stroke being the number 5 leading cause of death in the United States, the healthcare industry needs to be doing as much as it can to reduce the death numbers caused by stroke yearly. Although it’s a hard task for medical professionals to accurately pinpoint whether a stroke will occur and because many factors play into health, such as genetics, our group believes that implementing data analytics to create models will make the process of lowering stroke numbers easier. With quantitative data becoming more and more powerful in today’s age, we believe that our model will be a great starting point for the healthcare industry to understand patterns of stroke and lead to better, more effective models in the future to bring down the year stroke numbers in the United States.
