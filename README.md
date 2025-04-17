# Sales Forecasting for German Drug Store Chain

---

## Project Overview

Accurate sales forecasting is crucial for retailers to optimize inventory management, resource allocation, and customer satisfaction. In this project, we build a predictive model to forecast six weeks of daily sales for a drug store chain in Germany (Rossmann), leveraging historical sales data, store metadata, promotions, holidays, and derived features.

---

## Contents

- [Data](#data)  
- [Preprocessing & Cleaning](#preprocessing--cleaning)  
- [Feature Engineering](#feature-engineering)  
- [Exploratory Data Analysis](#exploratory-data-analysis)  
- [Modeling](#modeling)  
- [Results & Evaluation](#results--evaluation)  
- [Project Structure](#project-structure)  
- [Dependencies](#dependencies)  

---

## Data

1. **Store Metadata (`store.csv`)**  
   - Store type, assortment, competition distance & opening dates, Promo2 details.  
2. **Training Sales Data (`train.csv`)**  
   - Historical daily sales, customers, promotions, holidays, dates.  
3. **Test Data (`test.csv`)**  
   - Future dates with metadata; no sales or customer columns.

---

## Preprocessing & Cleaning

- **Missing Values**  
  - Imputed `CompetitionOpenSinceMonth` & `CompetitionOpenSinceYear` by mode grouped on distance.  
  - Filled Promo2-related fields with zero for non-participating stores.  
- **Outliers**  
  - Inspected `CompetitionDistance` and used median imputation for robustness.  
- **Date Parsing & Filtering**  
  - Converted date columns to `datetime`.  
  - Removed rows where stores were closed (`Open == 0`) before modeling.

---

## Feature Engineering

- **Season**: Mapped month to Winter, Spring, Summer, Fall.  
- **Competition Duration**: Calculated months since competition opened.  
- **Promo Years**: Number of years a store has participated in Promo2.  

---

## Exploratory Data Analysis

- **Weekly & Seasonal Trends**  
  - Identified weekday effects and seasonal fluctuations.  
- **Promotions & Holidays**  
  - Quantified uplift from promotions and varying holiday impacts.  
- **Store Characteristics**  
  - Compared sales by store type and assortment categories.  
- **Correlation Analysis**  
  - Highest correlations: Customers (`0.82`), Promo (`0.37`) and DayOfWeek (`‑0.18`).
 
You can browse all of the key EDA charts here:
[Visualizations.pdf](Visualizations.pdf)

---

## Modeling

### Model Selection  
We use a **Random Forest Regressor** because it:  
- Handles both numerical and categorical features without extensive preprocessing  
- Is robust to outliers (builds many decision trees rather than fitting exact points)  

### Feature Selection  
Based on correlation analysis and domain knowledge, we keep the following predictors:  
- `DayOfWeek`  
- `CompetitionDistance`  
- `competition_duration`  
- `StateHoliday`  
- `Promo`  
- `StoreType`  
- `Assortment`  

### Train–Validation Split  
- **Training set:** 80% of the merged data  
- **Validation set:** 20% of the merged data  
- **Random state:** 42 (for reproducibility)  

### Model Evaluation
We evaluated two different approaches for forecasting sales:

1. **Validation RMSE:**  
   This metric indicates the average error between the predicted sales and the actual sales in the validation set.
   A lower RMSE signifies better model performance, meaning the model's predictions are closer to the actual values
   
2. **Validation Accuracy (R² Score):**  
   The R² score represents the proportion of variance in sales explained by the model.
   A high R² score demonstrates that the model effectively captures the key patterns and relationships in the data, making it suitable for sales forecasting
   
### Test Predictions  
After training, we apply the model to the test set (with the same feature set) to generate six weeks of daily sales forecasts.

---

## Results & Evaluation

We evaluated two different approaches for forecasting sales:

1. **Direct Sales Prediction**  
   Predict sales directly using available features (excluding customers in the test set).  
   - Validation RMSE: 1,446.23  
   - Validation R²: 78.31%

2. **Two‑Step Prediction**  
   First predict customer counts, then use those predictions to forecast sales.  
   1. **Customer Prediction**  
      - Validation RMSE: 122.18  
      - Validation R²: 0.91  
   2. **Sales Prediction**  
      - Validation RMSE: 576.73  
      - Validation R²: 0.9655

The model effectively captures key drivers and delivers accurate six-week sales forecasts.

---

## Project Structure

| Path                                  | Description                                  |
|---------------------------------------|----------------------------------------------|
| `README.md`                           | Main project overview                        |
| `Project_File.ipynb`                  | Original analysis & modeling notebook        |
| `Dataset.zip/`                        | Directory containing data files              |
| `data/store.csv`                      | Store data                                   |
| `data/train.csv`                      | Historical sales training data               |
| `data/test.csv`                       | Future dates (test) dataset                  |


---

## Dependencies

- Python 3.8+  
- pandas  
- numpy  
- matplotlib  
- seaborn  
- scikit-learn  
- jupyter  

---


