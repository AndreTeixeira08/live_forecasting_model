# Project II – Predictive Model for Stock Management

## 🎯 Objective

Forecast weekly product sales to optimize stock replenishment for four selected stores in Istanbul. The aim is to reduce stockouts and overstocking using time series models and engineered features that reflect business and temporal dynamics.

---

## 🧱 Architecture (Microsoft Fabric)

The project was developed using a **Lakehouse architecture** in Microsoft Fabric, structured into:

- **Bronze Layer**: Raw data ingestion and conversion to Delta format
- **Silver Layer**: Data cleaning, normalization, and enrichment
- **Gold Layer**: Dimensional modeling using a star schema
- **ML Area**: Feature engineering and machine learning models
- **Power BI**: Visualization of results and reporting

---

## 🔍 Project Stages

### 1. 🔄 Data Pipeline

#### Bronze Layer
- Notebook: `Transform_to_Delta_Bronze_Notebook.ipynb`
- Converted raw data files into Delta format in `Projeto_II_Bronze_`

#### Silver Layer
- Notebook: `Cleansing_Silver_Notebook.ipynb`
- Applied data cleaning: handled nulls, standardized formats, fixed inconsistent codes (e.g., store/city codes)

#### Gold Layer
- Notebook: `Star_Schema_Gold_Notebook.ipynb`
- Created a star schema:
  - `dim_stores`
  - `dim_products`
  - `dim_dates` (with holiday/weekend flags)
  - `fact_sales`

---

### 2. 🧪 Exploratory Data Analysis (EDA)

Notebooks:
- `EDA_Full_Dataset_Initial_Insights.ipynb`
- `EDA_PoC_Initial_Insights.ipynb`
- `EDA_Define_PoC.ipynb`

Key insights:
- Defined the **Proof of Concept (PoC)** with 4 Istanbul stores (one per store type: ST01 to ST04)
- Focused on products from categories `hierarchy1_id` ∈ [H00, H01, H02, H03]
- Detected seasonality, stockout patterns, outliers, and temporal trends
- Identified missing data patterns and selected relevant variables for modeling

---

### 3. 🏗 Feature Engineering

Notebook: `Projeto_II_features_table.ipynb`

Engineered variables:
- `weekly_sales` – average product sales per week
- `avg_stock` – weekly average available stock
- `avg_price` – average price during the week
- `promo_bin_*_rate` – proportion of time in promotions
- `lag_sales_1w`, `lag_sales_2w` – previous weeks' sales to capture autoregressive effects
- `is_weekend`, `is_holiday` – calendar-based flags
- `num_obs` – number of transactions per week

These features were used as input for time series and regression models.

---

### 4. 📈 Predictive Models

Notebooks:
- `ML_ARIMA.ipynb`
- `ML_SARIMA.ipynb`
- `ML_SARIMAX.ipynb`
- `ML_REG_LIN_ML2.ipynb` (and test variant)

Models used:
- **ARIMA** – baseline model without seasonality or exogenous variables
- **SARIMA** – includes weekly seasonality
- **ARIMAX/SARIMAX** – adds exogenous variables (e.g., `avg_stock`, `lag_sales_1w`)
- **Linear Regression** – serves as benchmark using manually engineered features

Models were tested by `product_id` across the selected stores and hierarchies. Evaluation results were logged using Microsoft Fabric’s **ML Experiments**.

---

### 5. 📊 Reports and Outputs

Power BI Reports:
- `Projeto_II_Forecast_Report` – Final model forecasts and performance

Output files:
- Weekly forecasts and model evaluation results exported to `.csv` for further analysis

---

## 📂 Suggested GitHub Folder Structure

```
project_II_stock_forecasting/
│
├── notebooks/
│   ├── 01_eda_full_dataset.ipynb
│   ├── 02_eda_define_poc.ipynb
│   ├── 03_bronze_transform.ipynb
│   ├── 04_silver_cleaning.ipynb
│   ├── 05_gold_modeling.ipynb
│   ├── 06_feature_engineering.ipynb
│   ├── 07_model_arima.ipynb
│   ├── 08_model_sarima.ipynb
│   ├── 09_model_sarimax.ipynb
│   ├── 10_model_linear_regression.ipynb
│
├── reports/
│   ├── project_II_presentation.pdf
│   └── project_II_final_report.pdf
│
├── outputs/
│   └── forecasts_per_product.csv
│
├── data/
│   └── README.md  # How to access Fabric data (without uploading raw files)
│
├── requirements.txt
├── .gitignore
└── README.md
```

---

## 📐 Model Evaluation

Models were evaluated using:

- **MAE** – Mean Absolute Error  
- **AIC / BIC** – Model fit comparison for ARIMA-type models  

Evaluation was done at the product level, grouped by store and hierarchy (`store_id`, `hierarchy1_id`).

---

## 🚀 Future Work

- Switch from `sales` to **demand estimation** when `stock = 0`  
- Host data in **Azure Data Lake Storage (ADLS Gen2)** for automated ingestion  
- Add **moving averages** and other time-window features  
- Deploy forecasting logic for continuous weekly updates  

---

## 👥 Team & Acknowledgments

Developed as part of the **Postgraduate Program in Analytics & Data Science**.

- **André Teixeira** – Feature Engineering & Time Series Modeling
- **Patrícia Pereira** – EDA...
- **Rodrigo Diogo** – Linear Regression ML Modelling
- **Vitor Pereira** – Fabric Medallion Architecture  
- [Add names of teammates and responsibilities if applicable]

---
