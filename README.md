# Customer Shopping Behaviour Analysis

An end-to-end analytics project on 3,900 retail customer transactions: data cleaning and feature engineering in **Python**, business analysis in **MySQL**, and an interactive dashboard in **Tableau**.

## Project Overview

The goal is to understand how different customer groups shop and spend, and what drives revenue. The project answers questions such as:

- How does spending differ by gender, age group and subscription status?
- Do discounts and shipping type affect purchase value?
- Which products rate highest, sell most in each category, or rely most on discounts?
- How are customers distributed across New, Returning and Loyal segments?

## Tech Stack

| Stage | Tools |
|---|---|
| Cleaning & feature engineering | Python, pandas |
| Database | MySQL, SQLAlchemy, PyMySQL |
| Analysis | SQL (CTEs, window functions, aggregations, CASE) |
| Visualisation | Tableau |

## Repository Structure

```
.
├── behaviour_analysis.ipynb          # Data cleaning, feature engineering, load to MySQL
├── customer_behaviour_mySQL_.sql     # 10 business questions answered in SQL
├── Customer_behaviour.twb            # Tableau workbook (dashboard)
├── customer_shopping_behavior.csv    # Raw dataset (3,900 rows, 18 columns)
├── Updated_customer_behaviour.csv    # Cleaned dataset (3,900 rows, 19 columns)
└── README.md
```

## Dataset

Each row is one customer's purchase. Key columns: `Customer ID`, `Age`, `Gender`, `Item Purchased`, `Category`, `Purchase Amount (USD)`, `Location`, `Size`, `Color`, `Season`, `Review Rating`, `Subscription Status`, `Shipping Type`, `Discount Applied`, `Promo Code Used`, `Previous Purchases`, `Payment Method`, `Frequency of Purchases`.

## Workflow

### 1. Data cleaning and feature engineering (`behaviour_analysis.ipynb`)

- Inspected the data with `info()`, `describe()` and null counts.
- Filled missing **Review Rating** values with the **median rating of each product category**.
- Standardised column names to `snake_case` and renamed `purchase_amount_(usd)` to `purchase_amount`.
- Created **`age_group`** (Young Adult, Adult, Middle-aged, Senior) by splitting age into quartiles with `pd.qcut`.
- Created **`purchase_frequency_days`** by mapping purchase frequency (Weekly = 7, Fortnightly = 14, Monthly = 30, Quarterly = 90, Annually = 365, and so on) to a number of days.
- Dropped **`promo_code_used`** because it duplicates `discount_applied`.
- Loaded the cleaned data into a MySQL table named `customer` and exported `Updated_customer_behaviour.csv`.

### 2. SQL analysis (`customer_behaviour_mySQL_.sql`)

| # | Question |
|---|---|
| 1 | Total revenue from male vs. female customers |
| 2 | Customers who used a discount but still spent above the average purchase amount |
| 3 | Top 5 products by average review rating |
| 4 | Average purchase amount: Standard vs. Express shipping |
| 5 | Do subscribers spend more? (customers, average spend, total revenue) |
| 6 | Top 5 products with the highest share of discounted purchases |
| 7 | Customer segments (New / Returning / Loyal) by previous purchases |
| 8 | Top 3 most purchased products within each category |
| 9 | Are repeat buyers (more than 5 previous purchases) likely to subscribe? |
| 10 | Revenue contribution of each age group |

### 3. Tableau dashboard (`Customer_behaviour.twb`)

Built from `Updated_customer_behaviour.csv`, with:

- **KPI cards:** customer count, purchase amount, review rating
- **Charts:** subscription status split, revenue/sales by age group, revenue/sales by category
- **Interactive filters:** subscription status, gender, category and shipping type

## How to Run

1. **Install dependencies**
   ```bash
   pip install pandas sqlalchemy pymysql
   ```
2. **Set up MySQL.** Create the database:
   ```sql
   CREATE DATABASE IF NOT EXISTS customer_behaviour_analysis;
   ```
3. **Run the notebook.** Open `behaviour_analysis.ipynb`, update the MySQL connection details and the output path, then run all cells. This creates the `customer` table.
4. **Run the SQL.** Execute `customer_behaviour_mySQL_.sql` in MySQL Workbench or any MySQL client.
5. **Open the dashboard.** Open `Customer_behaviour.twb` in Tableau Desktop. If the data source is not found, repoint it to `Updated_customer_behaviour.csv`.

> **Note:** Do not commit real database credentials. Read them from environment variables instead, for example `os.environ["MYSQL_PASSWORD"]`.

## Skills Demonstrated

Data cleaning, feature engineering, SQL (CTEs, window functions, conditional aggregation), Python-to-MySQL pipelines, KPI design and interactive dashboards.

## Author

**Shwettam Sahu**, B.Tech in Information Technology, IIIT Bhubaneswar
