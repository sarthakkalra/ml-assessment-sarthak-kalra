# Part B: Business Case Analysis

**Author:** Sarthak Kalra  
**Assessment:** Machine Learning Assessment  

---

## B1. Problem Formulation

### (a) ML Problem Definition

**Target Variable:**  
- Number of items sold (sales volume) per store per month

**Input Features:**  
- Store attributes: location type (urban/semi-urban/rural), store size, footfall, competition density  
- Promotion type: Flat Discount, BOGO, Free Gift, Category-Specific Offer, Loyalty Points Bonus  
- Time-based features: month, seasonality, festivals, weekends  
- Historical performance: past sales, past promotion effectiveness  
- Customer demographics: income levels, preferences  

**Type of ML Problem:**  
- Supervised Learning – Regression  

**Justification:**  
The goal is to predict a continuous numerical value (items sold). Regression models are suitable for estimating numerical outcomes based on input features.

---

### (b) Why “Items Sold” is Better than Revenue

Revenue can be misleading because:
- Promotions directly affect pricing (e.g., discounts reduce revenue per item)  
- Higher revenue does not necessarily reflect higher customer engagement  

Items sold is a better metric because:
- It directly measures customer response  
- Reflects demand more accurately  
- Aligns with inventory movement and growth objectives  

**Broader Principle:**  
The target variable must align with the actual business objective. Choosing an incorrect target can lead to misleading model outcomes.

---

### (c) Alternative Modelling Strategy

Instead of using a single global model:

**Recommended Approach:**
- Segment-based modelling (e.g., urban vs rural stores)  
- Cluster stores based on similarity and train separate models  
- Use interaction features between store type and promotion  

**Justification:**  
Different stores respond differently to promotions. A single model may fail to capture local variations, whereas segmented models improve prediction accuracy.

---

## B2. Data and EDA Strategy

### (a) Data Joining and Dataset Design

**Data Sources:**
- Transactions  
- Store attributes  
- Promotion details  
- Calendar data  

**Join Strategy:**
- Join transactions with store attributes using `store_id`  
- Join promotions using `promotion_id`  
- Join calendar data using `date`  

**Final Dataset Grain:**  
- One row = one store per month per promotion  

**Aggregations:**
- Total items sold  
- Total number of transactions  
- Average basket size  
- Average revenue per transaction  

---

### (b) EDA Strategy

1. **Promotion vs Sales**
   - Chart: Bar plot of promotion type vs average items sold  
   - Purpose: Identify most effective promotions  

2. **Time Trends**
   - Chart: Line plot of monthly sales  
   - Purpose: Detect seasonality and trends  

3. **Store Type Analysis**
   - Chart: Boxplot of sales by store type  
   - Purpose: Understand location-based differences  

4. **Correlation Analysis**
   - Chart: Heatmap of numerical features  
   - Purpose: Identify key drivers of sales  

5. **Promotion Effectiveness by Store Type**
   - Chart: Grouped bar chart  
   - Purpose: Analyze how promotions perform across different store categories  

---

### (c) Handling Imbalance (80% No Promotion)

**Issue:**
- Model may become biased toward “no promotion”  
- Reduced ability to learn promotion impact  

**Solutions:**
- Oversample promotion data  
- Use stratified sampling  
- Introduce a promotion flag feature  
- Use appropriate evaluation metrics  

---

## B3. Model Evaluation and Deployment

### (a) Train-Test Strategy and Metrics

**Why Random Split is Inappropriate:**
- Breaks temporal sequence  
- Causes data leakage  
- Produces unrealistic performance  

**Recommended Approach:**
- Time-based split  
  - Training: First ~2.5 years  
  - Testing: Last ~6 months  

**Evaluation Metrics:**

- **MAE (Mean Absolute Error):**  
  Average prediction error in number of items sold  

- **RMSE (Root Mean Squared Error):**  
  Penalizes large errors more heavily  

- **R² Score:**  
  Measures how well the model explains variance  

---

### (b) Explaining Model Recommendations

**Approach:**
- Use feature importance from the trained model  
- Compare feature values for different months  

**Example:**
- December: High demand due to festivals → Loyalty Points Bonus is effective  
- March: Lower demand → Flat Discount helps drive sales  

**Communication:**
The model recommends different promotions for the same store because customer behavior varies across time, influenced by seasonal and contextual factors.

---

### (c) Deployment Strategy

**1. Model Saving**
- Save trained model using serialization (e.g., joblib)

**2. Monthly Prediction Pipeline**
- Collect new monthly data  
- Apply same preprocessing steps  
- Load saved model  
- Generate predictions for all stores  

**3. Automation**
- Schedule pipeline using tools like cron or workflow schedulers  
- Run predictions at the start of each month  

**4. Monitoring**
- Track prediction error over time  
- Monitor data drift (changes in feature distributions)  
- Evaluate business impact of recommendations  

**5. Retraining Triggers**
- Significant drop in model performance  
- Changes in customer behavior  
- Introduction of new promotions  

---
