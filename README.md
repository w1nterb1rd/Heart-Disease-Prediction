# Heart-Disease-Prediction

## Table of Contents
* [Overview](#overview)
* [Understanding the Data](#data)
* [Tools Used](#tools)
* [Cleaning the dataset](#clean)
* [Checking for correlation](#correlation)
* [Treating outliers](#outliers)
* [Dividing data into test and train](#division)
* [Modelling](#model)
* [Accuracy check](#accuracy)
* [Insights](#insight)
* [Alternative approach](#alt)
---

## Overview <a name="overview"></a>
<p align=justify> This dataset is taken from Kaggle. The aim of the analysis is to predict whether a person will suffer from heart disease or not. Since the prediction is to be made in a yes/no form, the dependent variable is categorical and the given dataset can be analysed using logistic regression method or random forest method. Logistic Regression is a statistical model used for binary classification, that is prediction of the type this or that, yes or no, A or B, etc. For binary data, the goal is to model the probability p that one of two outcomes occurs. </p>
<p align=center> ln[p/(1-p)] = b0 + b1X1 + b2X2 + … + bkXk </p>
<p align=justify> In Logistic Regression, we use maximum likelihood method to determine the best coefficients and eventually a good model fit. </p>


## Understanding the Data <a name="data"></a>
The dataset was loaded in R using the read.csv command and viewed using the View command. The dataset consists of 14 columns and data of 303 individuals. The variables can be explained as in the following table:-
| Syntax | Description |
| ----------- | ----------- |
| age | Age of the individual |
| sex | gender of the individual (0 = female, 1 = male) |
| cp | Chest pain type <br> (0 = typical angina, <br> 1 = atypical angina, <br> 2 = non-anginal pain, <br> 3 = asymptomatic) |
| trestbps | Resting blood pressure value of individual in mmHg |
| chol | Serum cholesterol level in mg/dl |
| fbs | Fasting blood sugar value of individual <br> (compared with 120mg/dl; If fasting blood sugar > 120mg/dl, then 1, else 0) |
| restecg | Resting electrocardiographic results <br> (0 = normal <br> 1 = having ST-T wave abnormality <br> 2 = left ventricular hyperthrophy) |
| thalach | Maximum heart rate achieved by an indiviual |
| exang | Exercise induced angina (1 = yes, 0 = no) |
| oldpeak | ST depression induced by exercise relative to rest |
| slope | Peak exercise ST segment (1 = upsloping, 2 = flat, 3 = downsloping) |
| ca | Number of major vessels |
| thal | Thalassemia (1 = normal, 2 = fixed defect, 3 = reversible defect) |
| target | Whether individual is suffering from heart attack or not <br> (0 = absence, 1 = present) |

### Explaination of terms:-
Clinical classification of chest pain (adapted from Braunwald et al (9))
 + Typical angina (definite) : 
   1. Substernal (below or behind the sternum) chest discomfort with a characteristic quality and duration that is
   2. Provoked by exertion or emotional stress
   3. Relieved by rest or nitroglycerine (medicine for heart attack)       
 + Atypical angina (probable) : Meets two of the above characteristics.
 + Non – anginal pain : Meets one or none of the above characteristics.

* Serum Cholesterol level : Measurement of the amount of high and low-density lipoprotein cholesterol (HDL and LDL) and the amount of triglycerides in a person's blood.

* ST-T wave abnormalities in resting ecg : see diagram <br>
![image](https://user-images.githubusercontent.com/109220216/178790625-78b90a05-5361-4d9a-bf0c-2e6e3d9bbbff.png)

## Tools Used <a name="tools"></a>
* We used R programming to analyse the given data.
* We have installed and used the following packages:
  + bit64
  + caret
  + data.table
  + usdm
* glm() function is used to build the Logistic Regression model. 


## Cleaning the dataset <a name="clean"></a>
<p align=justify> The first step towards analysing the data is ensuring that it is usable. This can be done by classifying the columns into required data types, creating any required variables, checking for NA values and omitting or substituting them, removing outliers, etc. This is one of the most important steps and must be completed before model-making to generate an accurate model. </p>

### Changes made to variables 
* The classes of the following variables are converted into factors to accurately define their content-
   + sex 
   + cp
   + Fbs
   + restecg 
   + exang 
   + slope 
   + ca 
   + thal
   + target
* Variable names are changed for better understanding
* A new variable named ‘age_Category’ is created for categorising the age for better understanding of data and easier analysis
* NA values are checked. No NA value present in data. <br>

![Picture1](https://user-images.githubusercontent.com/109220216/178794810-69786262-53a8-4a96-b60b-ca1747fa72b3.png)
![Picture2](https://user-images.githubusercontent.com/109220216/178794933-0b4c7152-e2f4-4d29-b5a8-f0af57a30db6.png)

## Checking for correlation <a name="correlation"></a>
The existence of a relationship between two or more variables or factors where dependence between them occurs in a way that cannot be attributed to chance alone. In R, methods to check for correlation:
1. Plot function
2. Pearson correlation coefficient for correlation between numeric variables.
3. Spearman correlation coefficient for correlation between factor variables.
 	
We have checked  for correlation between the following variables using cor() function: <br>

![Picture3](https://user-images.githubusercontent.com/109220216/178795812-5ff59f66-ce4b-4f7d-9447-1d1bd8676eae.png)
<br>

Next, we install the ‘corrplot’ package to obtain the visual representation of correlation. Positive correlations are displayed in blue and negative correlations in orange color. Color intensity and the size of the circle are proportional to the correlation coefficients. Bigger size of circle shows higher correlation. <br>

![Picture4](https://user-images.githubusercontent.com/109220216/178796202-b460e1e4-f475-4d4c-91c3-8ae5e25d20cc.png)
<br>

## Treating outliers <a name="outliers"></a>
<p align=justify> We install the package ggplot2. ggplot2 is a system for creating graphics, based on The Grammar of Graphics. We provide the data, tell ggplot2 how to map variables to aesthetics, what graphical primitives to use, and it takes care of the details. We have created boxplots for the attributes age, oldpeak, thalach, trestbps and chol with respect to age_Category to find any outliers in their data columns which might lead to incorrect results. We assume that values in a dataset are clustered around some central value. Any value that lies more than one and a half times the length of the box from either end of the box is an outlier. If a data point is below Q1 – 1.5×IQR or above Q3 + 1.5×IQR, it is an outlier. Outlier treatment by either removing them if they are not many in number otherwise imputed with the mode or median. </p>

## Dividing data into test and train <a name="division"></a>
<p align=justify> The entire dataset is randomly divided into two parts- train and test
Train: This part of the dataset is used for model building. Analysis is done for this dataset and an appropriate model is built according to requirements. <br>
Test: This part of the dataset is used to test the model on. After getting the output via the model on this data, it can be compared to the original output and the accuracy of the original model can be predicted </p>

### Tools used for dividing:-
* caret library is used for division of data.
* createDataPartition() function is used to divide the data.
* The dependent variable , i.e., ‘target’ is used as the basis of the division in order to avoid any biasness.
* 60% of data is taken for the train dataset and the rest 40% of data is taken for the test dataset. 

## Modelling <a name="model"></a>
<p align=justify> We have used Logistic Regression because the dependent variable (whether a person suffers from heart disease or not) is categorical, that is it has values 0- person does not suffers from heart disease OR 1- person suffers from heart disease. For better accuracy it is required that the variables do not have perfect correlation among themselves. Hence we check if multicollinearity exists, that is, we find the Variance Inflation Factor (VIF) for the variables in the data. If the VIF for a variable is greater than 5, it indicates a problematic amount of collinearity. In the dataset, the VIF for ‘age’ is very high, hence it should not be included in the model. </p>

Testing significance of regressors: We build a model by regressing ‘target’ with all other variables. Thereafter, we check the summary of the model built. <br>
Null Hypothesis (h0): The explanatory variable does not affect the dependent variable. <br>
Alternative Hypothesis (hA): The explanatory variable affects the dependent variable. <br>
If p>0.05 (level of significance), h0 is accepted, else it is rejected. <br>
* p-values for ‘sex’, ‘cp’, ‘ca’, are less than 0.05, hence they must be included in the model.
* p-value for ‘thalach’ is slightly higher than 0.05
* p-values for all other variables are much higher than 0.05, hence they must not be included in the model

Based on this, we first build model A by regressing ‘target’ with ‘sex’, ‘cp’, ‘ca’, and ‘thalach’. We build another model B by regressing ‘target’ with ‘sex’, ‘cp’, ‘ca’, and by dropping ‘thalach’ to see the impact.

We use Stepwise Elimination to select the important variables:
Step 1: We build a model with x1 variable only. 
Step 2: We include x2 in the model and see if it is significant (R-square goes up & AIC goes down). If yes, we keep the new model. 
Step 3: After inclusion of the second variable see if the existing variable can be dropped in the presence of new variable. Exclude x1 from the model and see if it were insignificant (R-square goes down & AIC goes up). If yes, we keep the new model. 
Step 4: Repeat the same for all the remaining variables while keeping the latest model.
Stepwise Elimination  gives us the following important variables - slope, thal, oldpeak, cp, ca, sex. Hence we build model C with these variables.


## Accuracy check <a name="accuracy"></a>
1. Using Predicted == Actuals: <br> This involves comparison of true(actual) values and predicted values. According to this:-
  * Accuracy for Model A =  0.793388429
  * Accuracy for Model B =  0.785123966
  * Accuracy for Model C = 0.851239669
Accuracy of Model C is highest (85.12%), making it the best fit model.

2. Confusion Matrix: <br>
(𝑇𝑟𝑢𝑒 𝑝𝑜𝑠𝑖𝑡𝑖𝑣𝑒𝑠+𝑇𝑟𝑢𝑒 𝑁𝑒𝑔𝑎𝑡𝑖𝑣𝑒𝑠)/(𝑇𝑟𝑢𝑒 𝑝𝑜𝑠𝑖𝑡𝑖𝑣𝑒𝑠+𝑇𝑟𝑢𝑒 𝑁𝑒𝑔𝑎𝑡𝑖𝑣𝑒𝑠+𝐹𝑎𝑙𝑠𝑒 𝑝𝑜𝑠𝑖𝑡𝑖𝑣𝑒𝑠+𝐹𝑎𝑙𝑠𝑒 𝑁𝑒𝑔𝑎𝑡𝑖𝑣𝑒𝑠)
where,<br>
true positives (TP): Cases in which we predicted yes (they have the disease), and they do have the disease.<br>
true negatives (TN): We predicted no, and they don't have the disease.<br>
false positives (FP): We predicted yes, but they don't actually have the disease.<br>
false negatives (FN): We predicted no, but they actually do have the disease.<br>

![Picture5](https://user-images.githubusercontent.com/109220216/178800711-d1a89ce3-517a-4165-8325-dcb60c334d78.png)<br>

According to the confusion matrix, the accuracy of the model is (add from final code):- <br>
For model A = ((44+52) / (44+52+11+14)) = 0.79338 <br>
For model B = ((42+53) / (42+53+13+13)) = 0.78512 <br>
For model C = ((48+55) / (48+55+11+7)) = 0.85123 <br>
Model C has highest accuracy (85.1%). Model C is the best fit model.

3. Receiver Operator Characteristic (ROC) Curve <br>
The aim is to push the ROC Curve towards 1 (upper left corner). The higher the curve, the larger the area under the curve (auc), better the model. It suggests that the diseased and non diseased groups are well distinguished.
  * Auc for Model A =  0.8870523
  * Auc for Model B =  0.8586777
  * Auc for Model C = 0.9299989 <br>
  
![Picture6](https://user-images.githubusercontent.com/109220216/178802039-981f9ee5-a492-419c-be4c-4566e4a83a4e.png)
![Picture7](https://user-images.githubusercontent.com/109220216/178802106-bf2bac90-c6b8-4788-940d-d0eff745b36e.png)
![Picture8](https://user-images.githubusercontent.com/109220216/178802140-b190d785-8f8d-47d9-ac26-4333142af35c.png) <br>

## Insights <a name="insight"></a>
Exploration of the data indicated that an individual’s age, gender, chest pain type, cholesterol level, maximum heart rate, exercise induced angina, ST peak depression induced by exercise and slope of the peak exercise ST segment were possible useful features for predicting the presence of heart disease. ECG results and thallasemia diagnosis were also found to have a minor predictive power. Based on the results, some of the following measures can be taken:-
* Male targeted heart health programs/schemes.
* Manufacture of cholesterol reducing/ inhibiting products.
* Anaemia awareness and management programs, medicine production.
* Since typical angina is maximum observed in heart patients, production of nitroglycerol can be increased. Nitroglycerol is medicine for typical angina.

## Alternative approach <a name="alt"></a>
<p align=justify> Alternatively Random Forest can also be used for model making. But, Random Forest being a ‘black box model’ , it is rather difficult to understand the underlying processes. Hence, we prefer to use Logistic Regression for modeling. </p>

















