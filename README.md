<h1 align="center">Terry-Stops-Classifier</h1>
<p align="center"><strong>A predictive model for arrests during SPD Terry Stops</strong></p>

![Terry Stop](https://github.com/user-attachments/assets/18183e38-5330-4c28-a096-24ff40b06bda)

## Overview
Terry Traffic Stops born from the landmark Supreme Court Case [Terry V. Ohio](https://www.oyez.org/cases/1967/67) allow police officers to stop suspicious drivers and even detain them on basis of 'reasonable suspicion.' However, this had led to inefficient resource utilization from unecessary stops in America based on the observed Stop-Arrest conversion rate. 
This project builds a predictive model to determine the likelihood of an arrest during Terry Stops conducted by the Seattle Police Department (SPD). Its goal is to identify the most influential factors that lead to arrests and further highlight the potential for racial profiling and the subjective nature of 'reasonable suspicion.'

## Business Understanding
### Stakeholders
The Seattle Police Department (SPD)

### Business Problem
The Seattle Police Department records show a high imbalance in the number of arrests made from Terry Stops where most result in non-arrests. This has contributed to inefficient resource allocation and continued violation of the Fourth Amendment. SPD cannot improve arrest scores without understanding the temporal, demographic, and call-type features that result in Terry Stops.
The  wants to understand what factors contribute to arrests during Terry Stops.

To address this, I aim to:
1. Identify the key determinants influencing arrests from Terry Stops.
2. Predict Terry Stops.
3. Provide actionable insights that enable SPD to implement targeted interventions.

**Overall Goal**: Reduce unnecessary stops and enhance resource allocation.  

**Data Science Objective**: Build a classification model to predict whether a stop leads to an arrest.

## Data Understanding:
The dataset used was is obtained from the [Seattle Police Department](https://data.seattle.gov/Public-Safety/Terry-Stops/28ny-9ts8/about_data). It contains arrest statistics and includes variables such as:

  1. Subject characteristics (race, perceived gender)
  2. Officer characteristics (race, age group, sex)
  3. Stop context (call type, reported year, weapon presence)
  4. Outcome of the stop (arrest or not)
  5. Reported time and date
  6. Unique identifiers: Subject ID, Officer ID, etc.
  7. Policing geographic information (Sector, Precinct, Beat)
     
This dataset is a historical record of Seattle Police arrests that span from the early 1900s to 2025. It describes officer and subject demographical characteristics, weapon type, call type and patrolling habits. The inclusion of sociodemographic and region-specific data helps highlight the effect of Terry Stops and their relationship to racial profiling and inefficient resource use.


## Exploratory Data Analysis
### a) Visualized class imbalance

<img width="589" height="457" alt="Distribution of Arrests" src="https://github.com/user-attachments/assets/cb8896f7-5ff4-4f94-8acf-cba6f7403c5e" />

**Insights:**
- There appears to be a massive class imbalance in the target variable. 
- Must keep this in mind for modeling stages and adjust accordingly.
- The trained model might end up predicting non-arrests better than arrests.

### b) Arrest rates by category

<img width="1589" height="1589" alt="Arrest rate by categorical columns" src="https://github.com/user-attachments/assets/e526d18a-3387-4fc8-8eb5-0a2ed644b462" />

**Insights:**
- There a lot more non-arrests than arrests in each category
- White officers have a lot more non-arrests and arrests
- Male officers have a lot more non-arrests and arrests
- The West Seattle precinct has the highest numbers of non-arrests and arrests
- Officers in their thirties had the highest numbers of non-arrests and arrests

**Target variable**: Arrest Flag (binary: 1 = arrest, 0 = no arrest)

## Data Preprocessing

    a) Handled missing values
    b) Categorical features: One-hot encoded or ordinal encoded
    c) Binary features: Mapped to 0/1
    d) Numerical features: Standardized
    e) Applied a unified ColumnTransformer to manage preprocessing inside pipelines

## Modeling 

I implemented and compared multiple classification models:

**1. Logistic Regression**
  - Started with a baseline model
  - Applied class balancing technique
  - Added regularization (L1, L2) to class_weight='balanced'
  - Built a final hyperparameter tuned model
 
**2. Decision Trees**
  - Started with a baseline model
  - Built a final hyperparameter tuned model
    
**3. Random Forest Classifier**
  - Tuned with GridSearchCV over 5-folds
  - Key hyperparameters: n_estimators=200, min_samples_leaf=2
      
## Feature Importance
1. Logistic Regression (Top Factors)
    - subject_age_group_25-44
    - call_type_Suspicious Circumstance
    - weapon_flag_True
      
<img width="977" height="435" alt="LR Feature Importance" src="https://github.com/user-attachments/assets/78048744-d812-4b63-8f2a-4163d3047226" />

2. Random Forest (Top Factors)
   - call_type and weapon_flag ranked consistently high
   - Showed interaction effects across multiple features

<img width="989" height="590" alt="RF Feature Importance" src="https://github.com/user-attachments/assets/af63e065-1af0-4cb2-af0d-6d3c9667e7d8" />

**Interpretation of Influential Features**  

**1. Common to Both Models**
  - Weapon Type Missing: Consistently important — absence may signal escalation risks or reporting issues.
  - Suspicious Circumstances / Call Type: Strongly associated with outcomes, likely because these are discretionary or higher-tension interactions.

**2. Only in Logistic Regression (directional insight)**
  - Race/Ethnicity: Some race features (like Hispanic or Multi-Racial) appear significant in LR but not in RF, suggesting they have predictive directionality but may not split the data cleanly.
    
**3. Only in Random Forest (nonlinear/interaction importance)**
  - Reported Year / Day / Month: Time-related features may not have linear relationships but are very helpful for trees.
  - Officer Age and Frisk Flag: Important in tree splits, possibly via interactions.
  
## Evaluation 

### 1. Classification Report
  **Logistic Regression**
   - Achieved ROC AUC: 0.9358
   - Provided interpretable feature importances
  
  **Decision Trees**
    - Performed poorer than Logistic Regression with tuned hyperparameters

  **Random Forest**
  - Achieved ROC AUC: 0.9470
  - Performed better on recall for arrest class (which is the minority class)
    
### 2. Overall Accuracy
**Logistic Regression:** Accuracy is 0.85, which is lower than the random forest's performance
**Random Forest:** Accuracy is 0.89 showing its benefit in handling imbalanced classes and datasets

### 3. ROC-AUC Score
**Logistic Regression: ROC-AUC = 0.9358**: The model is effective at distinguishing between arrest and non-arrest outcomes during Terry Stops. Its strong performance suggests that the linear relationship between features and the log-odds of arrest captures most of the predictive signal in the data. However, it may miss complex, nonlinear interactions between variables (e.g., interactions between subject race and call type).

**Random Forest: ROC-AUC = 0.9470**: Random forest does a better job capturing complex, nonlinear relationships and interactions among features compared to logistic regression or a single decision tree. Its high ROC-AUC score suggests excellent overall performance in distinguishing arrests.

### 3. ROC Curve

<img width="989" height="590" alt="ROC Curve Comparison" src="https://github.com/user-attachments/assets/d86e4070-cdbf-4357-ae43-275f05e03baf" />

**- Logistic Regression** has an ROC AUC of 0.9358 and its curve shows strong separation between classes, especially in early recall (low false positive rate).
It indicates good generalization but limited in capturing complex patterns.

**- Decision Tree** has an ROC AUC of 0.8985 which is lower than logistic and RF, suggesting reduced ability to balance sensitivity and specificity.It is however simpler and more interpretable, but prone to underfitting unless well-tuned.

**- Random Forest** has an ROC AUC of 0.9470. It is the best overall curve because it stays closest to the top-left corner. It indicates strong performance across thresholds due to ensembling and feature interaction handling.



## Conclusion
This analysis involved building and evaluating three classification models — Logistic Regression, Decision Trees, and a Random Forest Classifier — to predict the likelihood of an arrest occurring following a police dispatch. Logistic Regression provided valuable insights through its interpretable coefficients, highlighting features like “Weapon Type Missing”, subject race, and specific disturbance call types as influential predictors. However, the model struggled with recall on the minority class (arrests), despite a strong overall ROC-AUC of 0.9358.
The the Random Forest model not only enhanced predictive performance but also provided a richer understanding of the data through feature importance scores. These insights can inform policy reviews, officer training, and resource allocation strategies, especially regarding high-risk call types and demographic dynamics.

## Future Work
  - Try ensemble techniques like XGBoost or Gradient Boosting
  - Apply SHAP for better explainability
  - Deploy as a web-based prediction tool for field use

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
kevinjogu95@gmail.com  
[LinkedIn Profile](www.linkedin.com/in/kevin-karanja-njogu)
