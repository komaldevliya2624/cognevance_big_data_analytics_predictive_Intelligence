# 🛒 Big Data Analytics & Predictive Intelligence for E-Commerce

## 📌 Project Overview

**Big Data Analytics & Predictive Intelligence for E-Commerce** is an end-to-end data analytics and machine learning project designed to analyze e-commerce transaction data, identify business patterns, and forecast future sales.

The project combines **Python, Pandas, PySpark, MySQL, Scikit-learn, Matplotlib, and Power BI** to demonstrate a complete data analytics workflow — from data storage and cleaning to exploratory analysis, predictive modeling, visualization, and business insights.

---

## 🎯 Objectives

* Analyze e-commerce transaction data.
* Store and process data using **MySQL and PySpark**.
* Identify and handle invalid or inconsistent records.
* Perform feature engineering on transaction data.
* Analyze sales, customers, products, payment methods, and transaction status.
* Build a **sales forecasting model** using Machine Learning.
* Evaluate model performance using standard regression metrics.
* Generate future sales predictions.
* Create an interactive **Power BI dashboard**.
* Present actionable business insights from the data.

---

## 🧰 Technologies Used

| Technology       | Purpose                             |
| ---------------- | ----------------------------------- |
| Python           | Data analysis and programming       |
| Pandas           | Data manipulation and preprocessing |
| NumPy            | Numerical computation               |
| PySpark          | Big-data processing                 |
| MySQL            | Data storage and SQL analysis       |
| SQLAlchemy       | Python-MySQL connectivity           |
| PyMySQL          | MySQL database connection           |
| Scikit-learn     | Machine Learning                    |
| Matplotlib       | Data visualization                  |
| Jupyter Notebook | Development and analysis            |
| Power BI         | Interactive dashboard               |
| Git & GitHub     | Version control and project hosting |

---

## 📊 Dataset

The project uses an **e-commerce transaction dataset** containing transaction-level information.

### Dataset Features

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

Additional analytical features were created during feature engineering.

---

## 🔍 Data Quality & Cleaning

The original dataset contained several malformed or inconsistent records.

Validation was performed on:

* Product ID format
* Transaction date format
* Transaction status
* Payment method
* Quantity
* Unit price
* Total amount

Invalid records were removed rather than assigning fabricated values.

### Detected Invalid Records

| Column             | Invalid Records |
| ------------------ | --------------: |
| `product_id`       |             255 |
| `transaction_date` |             125 |
| `status`           |               7 |
| `payment_method`   |              31 |

After validation, the cleaned dataset contained:

**242 valid transactions**

### Valid Date Range

**January 1, 2023 – January 9, 2023**

The available valid data covered only one month, so monthly forecasting was not statistically suitable. Therefore, the forecasting analysis was performed at the **daily level**.

---

## ⚙️ Feature Engineering

The following features were generated from the transaction timestamp:

* `year`
* `month`
* `day`
* `day_of_week`
* `hour`
* `net_amount`

Where:

```text
net_amount = total_amount - shipping_cost
```

For forecasting, a numerical:

```text
time_index
```

was also created to represent the progression of days.

---

## 🚀 Project Workflow

```text
Raw E-Commerce Data
        ↓
MySQL Database
        ↓
Data Validation & Cleaning
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
Sales Forecasting
        ↓
Power BI Dashboard
        ↓
Business Insights
```

---

## 🗄️ MySQL Integration

The transaction data was imported into **MySQL** and accessed from Python using SQLAlchemy/PyMySQL.

Example workflow:

```python
from sqlalchemy import create_engine

engine = create_engine(
    "mysql+pymysql://username:password@localhost/database_name"
)
```

The data was then loaded into Pandas and subsequently converted into a PySpark DataFrame for distributed processing.

---

## ⚡ PySpark Processing

PySpark was used for scalable data processing and aggregation.

Example:

```python
daily_sales = transactions_ml.withColumn(
    "date",
    to_date("transaction_timestamp")
).groupBy("date").agg(
    spark_sum("total_amount").alias("sales")
).orderBy("date")
```

This generated daily sales data for forecasting.

---

## 📈 Exploratory Data Analysis

The project analyzes several important business dimensions:

### Sales Analysis

* Total revenue
* Average transaction value
* Daily sales trends
* Sales distribution

### Customer Analysis

* Customer-wise revenue
* Highest-value customers
* Customer purchasing patterns

### Product Analysis

* Product-wise revenue
* Top-performing products
* Product contribution to sales

### Payment Analysis

* Revenue by payment method
* Transaction distribution by payment method

### Transaction Status Analysis

* Completed transactions
* Pending transactions
* Cancelled transactions
* Refunded transactions

---

🤖 Machine Learning — Sales Forecasting

Because the cleaned dataset contained only one month of valid data, daily sales forecasting was selected instead of monthly forecasting.

A Linear Regression model was used as the baseline forecasting model.

Model Input
time_index
Target Variable
daily sales
Train-Test Split

The dataset was divided into training and testing sets using an 80:20 split while preserving chronological order.

X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    shuffle=False
)
Model
from sklearn.linear_model import LinearRegression

model = LinearRegression()

model.fit(X_train, y_train)

predictions = model.predict(X_test)
📏 Model Evaluation

The model was evaluated using:

MAE — Mean Absolute Error

Measures the average absolute difference between actual and predicted sales.

MSE — Mean Squared Error

Measures the average squared prediction error.

RMSE — Root Mean Squared Error

Measures prediction error in the same unit as the target variable.

R² Score

Measures how much of the variation in sales is explained by the model.

The final metric values are available in the project notebook.

🔮 Future Sales Forecast

The trained model was used to predict sales for the next 7 days.

future_indices = np.arange(
    last_index + 1,
    last_index + 8
).reshape(-1, 1)

future_predictions = model.predict(future_indices)

The predictions were exported as:

sales_forecast.csv
📊 Visualizations

The project includes visual analysis such as:

Daily sales trends
Actual vs predicted sales
Payment method revenue
Transaction status analysis
Customer revenue analysis
Product revenue analysis

Example forecasting visualization:

plt.plot(
    X_test["time_index"],
    y_test,
    marker="o",
    label="Actual Sales"
)

plt.plot(
    X_test["time_index"],
    predictions,
    marker="o",
    label="Predicted Sales"
)
📊 Power BI Dashboard

An interactive Power BI dashboard is planned to provide a business-friendly view of the analysis.

Key Dashboard KPIs
Total Revenue
Total Transactions
Average Transaction Value
Total Quantity
Shipping Cost
Net Revenue
Dashboard Visuals
Daily Sales Trend
Payment Method Analysis
Transaction Status
Top Customers
Top Products
Sales Forecast
📁 Project Structure
ecommerce-predictive-intelligence/
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
📦 Installation

Clone the repository:

git clone https://github.com/komaldevliya2624/ecommerce-predictive-intelligence.git

Move into the project directory:

cd ecommerce-predictive-intelligence

Install the required Python packages:

pip install -r requirements.txt
📋 Requirements
pandas<3
numpy
pyspark
pyarrow
scikit-learn
matplotlib
seaborn
sqlalchemy
pymysql
jupyter
▶️ How to Run
1. Start MySQL

Make sure MySQL is running and the project database is available.

2. Open Jupyter Notebook
jupyter notebook
3. Open
notebooks/ecommerce_predictive_intelligence.ipynb
4. Run the notebook cells sequentially

The notebook performs:

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
⚠️ Project Limitation

The cleaned dataset contains only 242 valid transactions covering January 1–9, 2023.

Because the available valid data covers only a short period, the sales forecasting model should be considered a baseline demonstration rather than a production-grade forecasting system.

A larger dataset covering several months or years would provide a stronger basis for reliable forecasting.

🔮 Future Improvements

Future versions of this project can include:

Larger historical datasets
Time-series models such as ARIMA, SARIMA, Prophet, or advanced ML models
Customer segmentation using clustering
Customer Lifetime Value prediction
Product recommendation system
Fraud/anomaly detection
Advanced demand forecasting
Real-time data processing using Spark Streaming
Automated ETL pipelines
Cloud deployment
Real-time Power BI dashboards
💡 Key Learning Outcomes

Through this project, I worked with:

Data cleaning and validation
Feature engineering
Pandas
PySpark
MySQL
SQL-based data storage
Exploratory Data Analysis
Machine Learning
Regression model evaluation
Sales forecasting
Data visualization
Power BI
GitHub project organization
👩‍💻 Author

Komal Devliya

BCA Graduate | Aspiring Data Analyst & Data Scientist

Areas of Interest
Data Analytics
Data Science
Machine Learning
Python
SQL
Power BI
Big Data Analytics
