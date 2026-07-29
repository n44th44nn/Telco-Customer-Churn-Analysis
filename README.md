# ☎️IBM Telco Customer Churn Analysis
Customer churn is a critical challenges in the telecommunication industry, where retaining existing users is significantly more cost-effective than acquiring new ones. This project leverages the IBM Telco Customer Churn dataset to uncover underlying behavioral patterns and predict potential churners.

---

## 📌Project Overview
### The Objective
The primary goal of this project is to analyze the IBM Telco Customer Churn dataset to uncover underlying behavioral patterns and predict potential churners. By maximazing the recall score for predicting the positive churn label, this machine learning model aims to catch as many potential churners as possible, enabling proactive retention strategies.

### The Approach
To build a robust prediction model, establishing a comprehensive data science pipelines is necessary. First, five seperate Excel files was merged into a cohesive master dataset. During early preprocessing, we conducted exploratory data analysis for find insights about customer churn behavior. To prepare the data for machine learning, we need to handle categorical feature, skewness, and data imbalance. Finally, we are going to train multiple advance machine learning algorithms that focuse on maximazing the Recall Score.

---

## 📊Dataset Information
The data from this project originates from [IBM Cognos Analytics](https://community.ibm.com/community/user/blogs/steven-macko/2019/07/11/telco-customer-churn-1113) sample datasets. It tracks customer churn, subscribed services, account information, and demographics for a fictional telecommunications company. I retrieved this specific version of the dataset from [Kaggle Telco customer churn (11.1.3+)](https://www.kaggle.com/datasets/ylchang/telco-customer-churn-1113). The raw data provided on Kaggle includes `Telco_customer_churn_demographics.xlsx`, `Telco_customer_churn_location.xlsx`, `Telco_customer_churn_population.xlsx`, `Telco_customer_churn_services.xlsx`, and `Telco_customer_churn_status.xlsx`.

### Data Integration Process
From those information, not all features were relevant for our customer churn analysis. Therefore, we need to carefully select features and remove redundant or uninformative data points before merging.

#### Features Removed
Several columns were excluded from the final master dataset as they did not contribute meaningful variance or were redundant:
* **Administrative & Single-Value Columns:** Feature like `Count` and `ID` were dropped as they offer no predictive value.
* **Redundant Spatial Data:** The `Lat Long` column was removed because the individual `Latitude` and `Longitude` columns already captured
* **Overlapping or Irrelevant Columns:** Features such as `Dependents` (redundant to `Number of Dependents`), `Quarter` (static time indicator), `Referred a Friend`, and `Offer` were dropped also.

#### Features Retained
The remaining 40 columns were carefully chosen to provide a holistic view of the customer across four key dimensions:
* **Customer Demographics:** `Age`, `Gender`, `Senior Citizen`, `Married`, etc., to identify if certain life stages correlate with churn.
* **Location & Population:** `City`, `Zip Code`, and `Population` density to capture regional behavioral trends.
* **Account & Service Usage:** `Internet Type`, `Tenure in Months`, `Contract type`, and `Monthly Charge`. These are critical indicators of customer engagement, financial commitment, and overall satisfaction.