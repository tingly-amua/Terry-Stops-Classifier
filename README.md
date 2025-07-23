# Terry-Stops-Classifier
A classifier to predict whether an arrest was made after a Terry Stop in Seattle.

## Overview

This project builds a predictive model to determine the likelihood of an arrest during Terry Stops conducted by the Seattle Police Department (SPD). The goal is to identify the most influential factors that lead to arrests and reduce unnecessary stops through data-informed policing strategies.

## Problem Statement

The SPD wants to understand what factors contribute to arrests during Terry Stops.

**Business Goal**: Reduce unnecessary stops and enhance resource allocation.  
**Data Science Objective**: Build a classification model to predict whether a stop leads to an arrest.

## Dataset:

The dataset used was provided by the [Seattle Police Department](https://data.seattle.gov/Public-Safety/Terry-Stops/28ny-9ts8/about_data), including variables such as:

  1. Subject characteristics (race, age group, perceived gender)

  2. Stop context (call type, reported year, weapon presence)

  3. Outcome of the stop (arrest or not)

**Target variable**: Arrest Flag (binary: 1 = arrest, 0 = no arrest)

## Data Preprocessing

    a) Handled missing values

    b) Categorical features: One-hot encoded or ordinal encoded

    c) Binary features: Mapped to 0/1

    d) Numerical features: Standardized

    e) Applied a unified ColumnTransformer to manage preprocessing inside pipelines

## Exploratory Data Analysis

Visualized class imbalance, arrest rates by category, and weapon presence.

<img width="1920" height="1440" alt="arrests_distribution" src="https://github.com/user-attachments/assets/851c655f-a423-4d1e-88b2-9fe26ad081d1" />


## Modeling Approach

I implemented and compared multiple classification models:
1. Logistic Regression

   - Applied regularization (L1, L2) with class_weight='balanced'

    - Achieved ROC AUC: 0.9358

    - Provided interpretable feature importances

2. Random Forest Classifier

    - Tuned with GridSearchCV over 5-folds

    - Key hyperparameters: n_estimators=200, min_samples_leaf=2

    - Achieved ROC AUC: 0.9470

    - Performed better on recall for arrest class (which is the minority class)

<img width="1920" height="1440" alt="roc_comparison" src="https://github.com/user-attachments/assets/c59c9026-c7b8-4f7b-b91a-c8cc7a985f54" />

## Evaluation Metrics
|Model	|ROC AUC	|Precision	|Recall |F1 Score|
|Logistic Regression	|0.9358	|0.54	|0.84	|0.65|
|Random Forest	|0.9470	|0.51	|0.87	|0.64|

1 = Arrest class (minority)

Confusion matrices and classification reports were generated for both models.

## Feature Importance

1. Logistic Regression (Top Factors)

    - subject_age_group_25-44

    - call_type_Suspicious Circumstance

    - weapon_flag_True

![Arrests Distributions](<img width="1920" height="1440" alt="logistic_regression_feature_importance" src="https://github.com/user-attachments/assets/5267b963-e401-44fc-93ae-9a9dff7c9ba5" />)

2. Random Forest (Top Factors)

    call_type and weapon_flag ranked consistently high

    Showed interaction effects across multiple features

(<img width="1920" height="1440" alt="random_forest_feature_importance" src="https://github.com/user-attachments/assets/45dcc9f5-b676-4604-9de7-8cc317b4d9b4" />)

## Conclusion

    - The models accurately predict the likelihood of arrest, especially Random Forest.

    - Key arrest predictors: Type of call, subject age group, perceived race, presence of weapon

    - Actionable insight: SPD can revise stop policies or better allocate patrol units based on stop context and risk level.

## Future Work

    - Try ensemble techniques like XGBoost or Gradient Boosting

    - Apply SHAP for better explainability

    - Deploy as a web-based prediction tool for field use

## Tools Used

    - Python (Pandas, Scikit-learn, Matplotlib, Seaborn)

    - Jupyter Notebook

    - GridSearchCV for tuning

    - ROC, F1, Precision-Recall metrics for evaluation

## Recommendations
1. **Enhance Officer Training to Reduce Bias in Arrest Decisions**
Implement scenario-based training that emphasizes de-escalation and checks implicit bias—especially in high-subjective-call scenarios—to reduce unnecessary arrests.

2. **Prioritize Deployment of Resources Based on High-Risk Call Types**
Use the model’s output to flag high-risk categories (like PROWLER, DV - ARGUMENTS) and assign experienced officers or support units (e.g., mental health responders) for more nuanced handling.

3. **Use Predictive Insights to Inform Policy and Oversight, Not On-the-Ground Decisions**
Leverage the model at a policy level to audit Terry Stops, identify patterns of over-policing, and shape future guidelines. For example, analyze why certain call types or conditions consistently lead to arrests and evaluate if those arrests align with department policy and community impact goals.

## Contact
For questions, feedback, or collaboration:

Kevin Karanja  
[kevinjogu95@gmail.com]  
[LinkedIn Profile](www.linkedin.com/in/kevin-karanja-njogu)
