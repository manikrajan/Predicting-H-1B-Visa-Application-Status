# Predicting-H-1B-Visa-Application-Status

## Abstract

In the United States of America, the H-1B visa program allows U.S. employers to employ workers from other countries who lack citizenship for specialty occupations. However, with an annual cap, the approval of a particular H-1B application is critical to secure or continue employment in the United States for foreign workers. Moreover, as an immigrant visa, H-1B is the initial step that would determine the number of applications that seek permanent residency through securing a Green Card in the future. This process incorporates a multitude of factors, meaning that by identifying the critical features of the applications which impact the approval process we can estimate whether an application would be accepted for either the H-1B visas. Our goal is to estimate the probability of a Visa approval based on the different features of the H-1B application.

## Introduction

The H-1B program allows employers in the United States to hire nonimmigrant workers in specialty occupations or as fashion models of distinguished merit and ability. Specialty occupations are those that require the application of a body of highly specialized knowledge and the attainment of at least a bachelor's degree or its equivalent.
When an employer wants to hire a nonimmigrant worker, there are several steps they need to follow, and most of these steps are established by the United States Department of Labor. These steps ensure, among other criteria, that employers will pay wages to the H-1B nonimmigrant workers that are at least equal to the actual wage paid by the employer to other workers with similar experience and qualifications for the job or the prevailing wage for the occupation in the area of intended employment.
Moreover, the employer must file an H-1B petition called an H-1B visa application. Therefore, the primary focus of this study is the H-1B visa approval process by the United States Citizenship and Immigration Services (USCIS). Because it is beneficial to both employer and employee to identify the factors that are improving the H-1B visa approval odds, as the continuation of the employment depends upon the approval of the H1B petition. Therefore, this study's main objective is to build a predictive model that will predict the probability of approving an H-1B visa petition. We explored and analyzed several data sources and tested modeling techniques to achieve this objective.
This study's primary data source comes from the U.S. Department of Labor's employment-based immigration data, which includes cumulative quarterly and fiscal year releases of program disclosure data and historical fiscal year annual program and performance report information. A more detailed description of the data and the exploratory data analysis is discussed in section 2.
Section 3 and Section 4 details modeling techniques and the outcome of each modeling technique. Finally, Section 5 discusses the conclusions and future research. 

## Data Description

Our data is provided by the Department of Labor and comes in the form of Excel workbook files containing H-1B and Green Card application information from the last five years. Each row in a workbook represents an applicant and contains the final application status, employer information, and applicant information, such as country of origin, and other field specific values, like area of work. In some cases there are over 100 different features available for an applicant and hundreds of thousands of rows, though some of the available features will be unimportant or empty values. An example of what some of the features look like once read into PySpark can be seen below.

![Alt text](/table1.jpg?raw=true "Title")

Two variables which appeared to be potentially impactful features are the SOC Code and the Employer State. We can see the top five most common SOC Codes in the figure below.

![Alt text](/EDA1.jpg?raw=true "Title")

We can see that around 25% of all Codes fall under 15-1132, which is Software Developer. The other four codes here are also software development related job areas, giving us an interesting insight into the skills sets of the workers. We can also see the distribution of States which are sponsoring the visa’s in the graph below.

![Alt text](/EDA2.jpg?raw=true "Title")

We can see that California sponsors the most visas, all five of these top states house large cities, which sponsor high amounts of technology related positions, in line with what we would expect given the most popular SOC Codes.

## Methods

Our final combined dataset had 16 features, and most were categorical. Hence we decided to build Logistic Regression, Decision Tree Classifier, and Random Forest Classifier, models

### Feature Selection

We dropped features with IDs and dates, like the CASE_NUMBER, RECEIVED_DATE, VISA_CLASS, and DECISION_DATE. The feature FULL_TIME_POSITION was dropped since it had too many NULL values. 

![Alt text](/table2.jpg?raw=true "Title")

The data in features, SOC_CODE, SOC_TITLE, and JOB_TITLE seemed highly correlated, so we retained only SOC_CODE from these features and dropped the rest from our models. For similar reasons, we ended up dropping EMPLOYER_CITY and retained EMPLOYER_STATE. The feature "EMPLOYER_NAME" had 233575 unique employers. We had to drop this feature because of too many categories. 
Finally, we ended up selecting five features to build our models. The feature CASE_STATUS with the H1B application approval status will be the target variable. The dependent variables will be the features SOC_CODE,  EMPLOYER_STATE, WAGE_RATE_OF_PAY_FROM, and PW_UNIT_OF_PAY. 

### Feature Preprocessing

The data in the features WAGE_RATE_OF_PAY_FROM and PW_UNIT_OF_PAY, which represents the pay, were in different units like Hourly, Bi-Weekly, Monthly, Yearly, etc. The data in these features were transformed to represent a single unit, which is yearly.

![Alt text](/table3.jpg?raw=true "Title")

The data in the target variable CASE_STATUS had to be cleaned because some values are in capital letters and some are in small letters. There were also applications with the status certified-withdrawn. These applications were withdrawn only after being approved. Therefore, we decided to change from certified-withdrawn to certified. In the end, we ended with three classes in the target variable: denied, certified, and withdrawn.

![Alt text](/table4.jpg?raw=true "Title")
 
We intended to develop our models using the essential concepts we learned in this course: Transformer, Estimator, and Pipeline. We used transformers like StringIndexer, OneHotEncoder, and Vector Assembler. Logistic regression, Decision Tree Classifier, and Random Forest Classifier were used as estimators. And a pipeline was used to build the models.
For instance, the features SOC_CODE and EMPLOYER_STATE that we used in our model are categorical. So we used transformers like StringIndexer and OneHotEncoder to convert categorical features into binary vectors. Then, using the Vector Assembler, a single feature was created from the list of the features and then fed into the MLlib algorithms.
Finally, we used a pipeline to sequence the above stages of building our models. Since different models were developed, the pipeline helped ensure the automation and repeatability of the transformations applied to the dataset. 

## Model Development and Selection

The dataset was split into a training and test data set. The models were trained using the training dataset and evaluated using the test dataset. Due to the categorical nature of the data, We decided to build Logistic Regression, Decision Tree Classifier, and Random Forest Classifier models.
Models were optimized and tuned using hyperparameter tuning and cross-validation. The data was divided into three folds for cross-validation. "ParamGridBuilder" was used to define a grid search over hyperparameters. In the case of logistic regression, the model was tuned with different values for the regularization parameter. In the case of the Decision tree classifier and Random Forest Classifier models, the model was tuned with different values for hyperparameters like MaxDepth and the number of trees.

### Model Evaluation and Performance

The performance of the models was evaluated against the test data. Accuracy in all our models is almost the same, 96.2%. The Random Forest Classifier had the perfect precision, 1.0, while others were also close. But the confusion matrix for the Random Forest Classifier had zeros for all the classes except for class 1 (Certified), which indicated an imbalance in the dataset.

![Alt text](/table5.jpg?raw=true "Title")

## Conclusions and Future Research

The main focus of building various classification models is to construct a method to predict the approval rate of the H-1B visa petitions. 
In particular, we built logistic regression, decision tree, and Random Forest models and compared their performance. We found that the accuracy in all our models is almost the same, 96.2%. Furthermore, the Random Forest Classifier had the perfect precision, 1.0, while others were also close. But the confusion matrix for the Random Forest Classifier had zeros for all the classes except for class 1 (Certified), which indicated an imbalance in the dataset.
For future research, we can extend this research to build models to predict the Green Card approval rate as well as more methodologies to, especially to achieve a higher accuracy rate for the data with imbalanced predictor variables.



