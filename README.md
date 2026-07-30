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

---

## 📈Exploratory Data Analysis
Our EDA phase served a dual purpose, which are understanding the underlying data distribution to inform necesary preprocessing techniques and extracting actionable business insights regarding chustomer churn behavior.

### Technical Data Assesment
By evaluating the data's structure, we can identify several preprocessing requirements:
* **Cardinality Analysis:** Evaluating the frequency of categorical variables to manage high cardinality. For instance, `City` feature contained many rare values, so we will group these rare values into an 'Others'.
* **Target Imbalance:** `Churn Label` confirmed a significant skew between retained and churned customers. We can handle this with using whether resampling techniques (SMOTE-NC) or Class Weighting method.
* **Skewness Shape:** I detected that the numerical features is not really have a perfect skew. We can conclude a data transformation techniques or build robust machine learning algorithms, such as Random Forest and Gradient Boosting.

### Key Business Insights
To obtain a focus analysis, we are going to analyze the data based on several hypotheses:
1.  Do customers cancel more frequently when their monthly bills cross a certain financial threshold compared to those on 1-year or 2-year contracts?
<div align="center">
  <img src="Visualizations/figure_1.png" alt="Churn by Contract Type" width="80%">
  <p><i>Figure 1: Customer Churn Rate by Contract Type</i></p>
</div>

> **Business Insight:** Pie chart tells us that churners are dominated by *Month-to-Month* contract around **88.6%**. This indicates that *Month-to-Month* customers are highly flexible to leave. In the other hand, Stayed Customers are more uniform, separated from *Month-to-Month* and *Two Year*.

<div align="center">
  <img src="Visualizations/figure_2.png" alt="Churn by Contract Type" width="80%">
  <p><i>Figure 2: Customer Churn Rate by Contract Type</i></p>
</div>

> **Business Insight:** Most of the churner are leaving when their pay bills are expensive. For *Month-to-Month*, customers tend to leave when their bills are around 80$. The other side, customers with *One Year* and *Two Year* contract usually don't easily break the long-term contract, unless their bills are around 90$ more expensive than *Month-to-Month* contract.

2. Do younger customers rely on the service more (or less) than older customers?
<div align="center">
  <img src="Visualizations/figure_3.png" alt="Customer Churn Rate by Age Group" width="80%">
  <p><i>Figure 3: Customer Churn Rate by Age Group</i></p>
</div>

> **Business Insight:** While the younger and middle-aged demographics have a churn rate of around 21% to 24%, the *Senior Citizen* churn rate spikes to **41.7%**. This means that nearly half of the senior demographic is leaving the company. Interestingly, both group *Under 30* and *30 to 64* maintain a solid retention rate of around 75% to 78%, indicating that customers don't slowly get more likely to leave as they age.

**New Hypotheses:** Are their household size trigerring Senior Citizen churning?
<div align="center">
  <img src="Visualizations/figure_4.png" alt="Senior Citizen Churn: Living Alone vs. With Dependents" width="80%">
  <p><i>Figure 4: Senior Citizen Churn: Living Alone vs. With Dependents</i></p>
</div>

> **Business Insight:** Senior that living alone have a nearly **50%** churn rate. This group is highly vulnerable, likely due to a combination of fixed incomes and lack of at-home technical support. Besides, when they live with others (like a spouse or younger dependents), the presence of other people using internet or phone lines drastically reduces the likelihood that the senior will cancel the service.

3. Does the way a person pays their bill affect how long they stay?
<div align="center">
  <img src="Visualizations/figure_5.png" alt="Customer Tenure Distribution by Payment Method" width="80%">
  <p><i>Figure 5: Customer Tenure Distribution by Payment Method</i></p>
</div>

> **Business Insight:** The buttom bulge of green violin indicates that the vast majority of customers who pay by *Mailed Check* have extremely short tenures (mostly under 10-15 months). However, the other methods (*Bank Withdrawal* and *Credit Card*) show two bulges at the bottom and top. These shape reveal that customers using these method fall into two main campus: those who churn immediately and those who stay for a very long time (around more than 65 months). This indicates that manual payment method (*Mailed Check*) is highly correlated with early churn, while the automatic one (*Bank Withdrawal* and *Credit Card*) perform equally well at maintaining high-tenure customers.

4. Does referring friends or family lock a customer into the ecosystem?
<div align="center">
  <img src="Visualizations/figure_6.png" alt="The Social Anchor Effect: Churn Rate vs. Number of Referrals" width="80%">
  <p><i>Figure 6: The Social Anchor Effect: Churn Rate vs. Number of Referrals</i></p>
</div>

> **Business Insight:** While the 0 referrals reveals a high churn rate (**32,6%**), the highest churn rate (**46.7%**) occurs at exactly 1 referrals, suggesting customers are abusing short-term referral promotion and leaving. This is likely because the company ran a campaign like "Refer a friend and get a free month.". However, from 4 referrals onward, the churn rate never goes above **8%**, eventually flatlining near **0%** for users with 8+ referrals.

5. Does giving a customer a refund actually save them, or is it just a precursor to them leaving away?
<div align="center">
  <img src="Visualizations/figure_7.png" alt="The Social Anchor Effect: The Refund Red Flag: Does giving a refund prevent churn?" width="80%">
  <p><i>Figure 7: The Refund Red Flag: Does giving a refund prevent churn?</i></p>
</div>

> **Business Insight:** While issuing refunds provides a slight 6.6% reduction in churn, the churn rate remains high with over 20% of refunded customers. This means providing a refund is **NO** associated with churn potential.

6. Are customers churning because they feel dissatisfied with certain services they subscribe to?
<div align="center">
  <img src="Visualizations/figure_8.png" alt="Connectivity Dissatisfaction" width="80%">
  <p><i>Figure 8: Connectivity Dissatisfaction</i></p>
</div>

> **Business Insight:** Whether a customer has phone service or not, the churn rate barely changes (24.9% vs. 26.7%). The same is true for multiple lines (25.0% vs. 28.6%). However, Customer with **Fiber Optic** internet are churning at a rate of **40,7%**. Fiber Optic internet is supposed to be the fastest and best product. The customer may be dissatisfied with this company's premium internet.

<div align="center">
  <img src="Visualizations/figure_9.png" alt="Connectivity Dissatisfaction" width="80%">
  <p><i>Figure 9: Connectivity Dissatisfaction</i></p>
</div>

> **Business Insight:** The most dramatic drops in churn come from **Online Security** (dropping from 31,3% to 14,6%) and **Premium Tech Support** (dropping from 31,2% to 15.2%). Having these services help customer to eliminate technical friction. In the other hand, while online backup and device protection plan reduce churn, the drops are mucn softer. Customers are care far more about their network actively working and being secure (Tech Support / Security) than they do about hardware warranties or cloud storage.

<div align="center">
  <img src="Visualizations/figure_10.png" alt="Connectivity Dissatisfaction=" width="80%">
  <p><i>Figure 10: Connectivity Dissatisfaction</i></p>
</div>

> **Business Insight:** Across all three streaming categories (TV, Movies, and Music), customers who subscribe these service don't churn at a drastical rate. Streaming services inflate the customer's monthly charge, driving customers to cancel this just because they want to find cheaper bundle. However, customers who are paying for **Unlimited Data** more sensitive to churn (**31,7%**)

---

## 🛠️Data Preprocessing

