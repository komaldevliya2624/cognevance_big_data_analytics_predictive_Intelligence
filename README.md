# 🛒 Big Data Analytics & Predictive Intelligence for E-Commerce

## 📌 Project Overview

**Big Data Analytics & Predictive Intelligence for E-Commerce** is an end-to-end data analytics and machine learning project developed to analyze e-commerce transaction data, identify business patterns, and forecast future sales.

The project demonstrates a complete data analytics workflow using **MySQL, Python, Pandas, PySpark, Scikit-learn, Matplotlib, and Power BI**.

The workflow covers data storage, data validation, cleaning, feature engineering, big-data processing, exploratory data analysis, machine learning, sales forecasting, visualization, and business insights.

---

## 🎯 Project Objectives

* Analyze e-commerce transaction data.
* Store and retrieve data using MySQL.
* Perform data validation and cleaning.
* Process data using PySpark.
* Perform exploratory data analysis.
* Engineer useful analytical features.
* Analyze sales, customers, products, payment methods, and transaction status.
* Build a machine learning model for sales forecasting.
* Evaluate model performance using regression metrics.
* Predict sales for the next 7 days.
* Develop an interactive Power BI dashboard.
* Generate meaningful business insights.

---

## 🧰 Technologies & Tools

| Technology       | Purpose                             |
| ---------------- | ----------------------------------- |
| Python           | Programming and data analysis       |
| Pandas           | Data manipulation and preprocessing |
| NumPy            | Numerical computation               |
| PySpark          | Big-data processing                 |
| MySQL            | Data storage and querying           |
| SQLAlchemy       | Database connectivity               |
| PyMySQL          | MySQL connection                    |
| Scikit-learn     | Machine Learning                    |
| Matplotlib       | Data visualization                  |
| Seaborn          | Statistical visualization           |
| Jupyter Notebook | Project development                 |
| Power BI         | Interactive dashboard               |
| Git & GitHub     | Version control                     |

---

## 📊 Dataset

The project uses e-commerce transaction-level data containing the following attributes:

* `transaction_id`
* `customer_id`
* `product_id`
* `transaction_date`
* `quantity`
* `unit_price`
* `total_amount`
* `discount_applied`
* `status`
* `payment_method`
* `shipping_cost`

Additional features were created during the feature-engineering stage.

---

## 🔍 Data Cleaning & Validation

The dataset was validated before performing analysis and machine learning.

The following fields were checked for invalid or malformed values:

* Product ID
* Transaction date
* Transaction status
* Payment method
* Quantity
* Unit price
* Total amount

### Invalid Records Identified

| Column             | Invalid Records |
| ------------------ | --------------: |
| `product_id`       |             255 |
| `transaction_date` |             125 |
| `status`           |               7 |
| `payment_method`   |              31 |

Instead of manually guessing or fabricating missing information, invalid records were excluded from the analytical dataset.

### Final Clean Dataset

**Valid transactions: 242**

### Valid Data Period

**January 1, 2023 to January 9, 2023**

The valid dataset covered only one month. Therefore, monthly forecasting was not appropriate, and the forecasting analysis was changed to a **daily sales forecasting approach**.

---

## ⚙️ Feature Engineering

The transaction timestamp was used to generate additional analytical features:

* `year`
* `month`
* `day`
* `day_of_week`
* `hour`
* `net_amount`

The `net_amount` feature was calculated as:

```text
net_amount = total_amount - shipping_cost
```

For sales forecasting, a sequential `time_index` was created to represent the progression of days.

---

## ⚡ PySpark Processing

PySpark was used to process and aggregate the cleaned transaction data.

Daily sales were calculated by grouping transactions by date:

```python
daily_sales = transactions_ml.withColumn(
    "date",
    to_date("transaction_timestamp")
).groupBy("date").agg(
    spark_sum("total_amount").alias("sales")
).orderBy("date")
```

The resulting daily sales data was then used for forecasting.

---

## 📈 Exploratory Data Analysis

The project performs analysis across multiple business dimensions.

### Sales Analysis

* Total revenue
* Average transaction value
* Daily sales
* Sales trends

### Customer Analysis

* Customer-wise revenue
* Top customers
* Customer purchasing contribution

### Product Analysis

* Product-wise revenue
* Top-performing products
* Product sales contribution

### Payment Analysis

* Revenue by payment method
* Transaction distribution by payment method

### Transaction Status Analysis

* Completed transactions
* Pending transactions
* Cancelled transactions
* Refunded transactions

---

# 🤖 Machine Learning — Sales Forecasting

## Model Selection

Since the cleaned dataset contains only one month of valid data, **daily sales forecasting** was used instead of monthly forecasting.

A **Linear Regression** model was implemented as a baseline forecasting model.

### Input Feature

```text
time_index
```

### Target Variable

```text
daily sales
```

### Train-Test Split

The data was divided chronologically using an **80:20 train-test split**.

```python
X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    shuffle=False
)
```

### Model Training

```python
from sklearn.linear_model import LinearRegression

model = LinearRegression()

model.fit(X_train, y_train)

predictions = model.predict(X_test)
```

---

## 📏 Model Evaluation

The model was evaluated using:

### MAE — Mean Absolute Error

Measures the average absolute difference between actual and predicted sales.

### MSE — Mean Squared Error

Measures the average squared prediction error.

### RMSE — Root Mean Squared Error

Measures prediction error in the same unit as the target variable.

### R² Score

Measures the proportion of variation in the target variable explained by the model.

The exact evaluation results are available in the project notebook.

---

# 🔮 Future Sales Forecast

The trained model was used to generate a **7-day future sales forecast**.

```python
future_indices = np.arange(
    last_index + 1,
    last_index + 8
).reshape(-1, 1)

future_predictions = model.predict(future_indices)
```

The forecast results were saved as:

```text
sales_forecast.csv
```

---

# 📊 Data Visualization

The project includes visualizations for:

* Daily sales trends
* Actual vs predicted sales
* Payment-method analysis
* Transaction-status analysis
* Customer revenue
* Product revenue
* Sales forecasting

### Actual vs Predicted Sales

The forecasting model compares actual test-set sales with model predictions to evaluate the model's performance.

---

# 📊 Power BI Dashboard

An interactive Power BI dashboard is included/planned as part of the project.

### Key KPIs

* Total Revenue
* Total Transactions
* Average Transaction Value
* Total Quantity
* Shipping Cost
* Net Amount

### Dashboard Analysis

The dashboard provides visual analysis of:

* Daily sales trends
* Payment methods
* Transaction status
* Top customers
* Top products
* Sales forecast

---

# 🗄️ Database Integration

The project uses **MySQL** for data storage.

The transaction data was accessed from Python using SQLAlchemy and PyMySQL.

Example:

```python
from sqlalchemy import create_engine

engine = create_engine(
    "mysql+pymysql://username:password@localhost/database_name"
)
```

The data was then loaded into Python and converted into a PySpark DataFrame for further processing.

---

# 🔄 Project Workflow

```text
Raw E-Commerce Dataset
        ↓
MySQL Database
        ↓
Data Loading
        ↓
Data Validation
        ↓
Data Cleaning
        ↓
Feature Engineering
        ↓
PySpark Processing
        ↓
Exploratory Data Analysis
        ↓
Daily Sales Aggregation
        ↓
Machine Learning
        ↓
Model Evaluation
        ↓
7-Day Sales Forecast
        ↓
Power BI Dashboard
        ↓
Business Insights
```

---

# 📁 Repository Structure

```text
cognevance_ecommerce-predictive-intelligence/
│
├── data/
│   ├── ecommerce_cleaned.csv
│   └── sales_forecast.csv
│
├── notebooks/
│   └── ecommerce_predictive_intelligence.ipynb
│
├── src/
│   ├── data_cleaning.py
│   ├── analysis.py
│   └── forecasting.py
│
├── dashboard/
│   └── ecommerce_dashboard.pbix
│
├── reports/
│   ├── project_report.pdf
│   └── project_presentation.pptx
│
├── requirements.txt
└── README.md
```

---

# 📦 Installation

Clone the repository:

```bash
git clone https://github.com/komaldevliya2624/cognevance_ecommerce-predictive-intelligence.git
```

Navigate to the project directory:

```bash
cd cognevance_ecommerce-predictive-intelligence
```

Install the required Python libraries:

```bash
pip install -r requirements.txt
```

---

# ▶️ How to Run the Project

### Step 1 — Start MySQL

Make sure your MySQL server is running.

### Step 2 — Open Jupyter Notebook

```bash
jupyter notebook
```

### Step 3 — Open the project notebook

```text
notebooks/ecommerce_predictive_intelligence.ipynb
```

### Step 4 — Run the notebook

Run the cells sequentially to perform:

```text
Database Connection
        ↓
Data Loading
        ↓
Data Validation
        ↓
Data Cleaning
        ↓
Feature Engineering
        ↓
PySpark Processing
        ↓
EDA
        ↓
Daily Sales Aggregation
        ↓
Model Training
        ↓
Model Evaluation
        ↓
7-Day Forecast
```

---

# ⚠️ Project Limitations

The final cleaned dataset contains **242 valid transactions** covering **January 1–9, 2023**.

Because the available valid historical data covers a relatively short period, the Linear Regression forecasting model should be considered a **baseline model for demonstration and learning purposes**, rather than a production-ready forecasting solution.

A larger dataset covering multiple months or years would allow more reliable forecasting and stronger model evaluation.

---

# 🔮 Future Scope

The project can be further improved by implementing:

* Larger historical datasets
* Advanced time-series forecasting
* ARIMA/SARIMA
* Prophet
* Random Forest
* XGBoost
* Customer segmentation
* Customer Lifetime Value prediction
* Product recommendation
* Fraud and anomaly detection
* Real-time analytics
* Automated ETL pipelines
* Cloud-based big-data processing
* Advanced Power BI dashboards

---

# 📚 Learning Outcomes

This project provided practical experience in:

* Data cleaning
* Data validation
* Feature engineering
* Pandas
* NumPy
* PySpark
* MySQL
* SQL/database integration
* Exploratory Data Analysis
* Machine Learning
* Regression
* Model evaluation
* Sales forecasting
* Data visualization
* Power BI
* GitHub project management

---

# 👩‍💻 Author

## Komal Devliya

**BCA Graduate | Aspiring Data Analyst & Data Scientist**

### Areas of Interest

* Data Analytics
* Data Science
* Machine Learning
* Python
* SQL
* Power BI
* Big Data Analytics

---

# 📌 Project Submission

This project has been developed as part of the **Cognevance project/internship submission**.

The repository contains the project source code, cleaned dataset, notebook, forecasting output, dashboard, documentation, and supporting files.

---

## ⭐ Conclusion

The project demonstrates an end-to-end approach to transforming raw e-commerce transaction data into meaningful analytical and predictive insights.

It combines database management, data cleaning, big-data processing, exploratory analysis, machine learning, forecasting, and business visualization into a single practical e-commerce analytics solution.

