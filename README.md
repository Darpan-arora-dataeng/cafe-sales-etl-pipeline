# Cafe Sales ETL Pipeline and Power BI Dashboard

## Project Overview

This project implements an ETL (Extract, Transform, Load) workflow using Python, Pandas, and PostgreSQL to process cafe sales data and perform SQL-based business analysis.

The project demonstrates data inspection, data quality checks, data cleaning, data transformation, database loading, and business reporting.

## Tech Stack

- Python
- Pandas
- NumPy
- PostgreSQL
- SQLAlchemy
- Jupyter Notebook
- pgAdmin

## Project Workflow

1. Extract sales data from a CSV file.
2. Inspect the dataset and identify data quality issues.
3. Clean missing values, invalid placeholders, and incorrect data types.
4. Transform the data and create additional analytical columns.
5. Load the transformed data into PostgreSQL.
6. Run SQL queries to analyze sales and business performance.
7. Document the findings in a project report.

## Project Structure

```text
P2 - Sales ETL Pipeline/
├── SQL/
├── .venv/
├── cafe_sales_dataset.csv
├── Sales.ipynb
├── sales_analysis.sql
├── README.md
└── sales_etl_report.md
```

*Note: Keep `sales_analysis.sql` in this structure only if that is the actual filename inside your SQL folder.*

## Dataset

- Source file: `cafe_sales_dataset.csv`
- Original records: 10,000
- Original columns: 8

The dataset contains transaction identifiers, items, quantities, unit prices, total amounts, payment methods, locations, and transaction dates.

## Business Analysis

The SQL analysis examines:

- Total revenue and average transaction value
- Total transactions and quantity sold
- Monthly and weekday sales trends
- Revenue and quantity by product
- Average selling price by product
- Payment method preferences
- Sales and average transaction value by location

## Key Findings

- Total recorded revenue: 88,952.00
- Highest-sales month: June 2023
- Highest-sales weekday: Thursday
- Highest-revenue identified item: Salad
- Highest-quantity identified item: Coffee
- Most frequently used identified payment method: Digital Wallet
- In-store sales were slightly higher than Takeaway sales.

These findings are based on the available data and are subject to the dataset's remaining missing values.

## How to Run

1. Clone or download the project.
2. Create and activate a Python virtual environment.
3. Install the required Python packages.
4. Open `Sales.ipynb` in VS Code and select the project's Python environment.
5. Run the notebook cells in order.
6. Configure PostgreSQL and update the database connection settings.
7. Execute the saved SQL queries in pgAdmin.

## Limitations

Some missing values remained after cleaning, including transaction dates and sales amounts. The dataset also lacks product costs and operating expenses, so profitability cannot be determined.

## Future Improvements

- Automate ETL execution.
- Add logging and error handling.
- Implement incremental loading and duplicate prevention.
- Add data-quality monitoring.
- Build a sales dashboard using Power BI.

## Author

Data Engineering Portfolio Project

 