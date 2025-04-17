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

---

## Modeling

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

---

## Results & Evaluation

- **Validation (Sales)**  
  - RMSE: 1,446.23  
  - R²: 88.31%  
- **Feature Importance**  
  1. Competition Distance (0.52)  
  2. Promo (0.17)  
  3. Promo Years (0.09)  

The model effectively captures key drivers—competition proximity and promotional activity—and delivers accurate six-week sales forecasts.

---

## Project Structure

| Path                                  | Description                                  |
|---------------------------------------|----------------------------------------------|
| `README.md`                           | Main project overview and instructions       |
| `Project_File.ipynb`                  | Original analysis & modeling notebook        |
| `Dataset.zip/`                            | Directory containing data files              |
| `data/store.csv`                      | Store metadata                               |
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


