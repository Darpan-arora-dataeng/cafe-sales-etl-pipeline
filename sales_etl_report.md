## Cafe Sales ETL Pipeline using Python and PostgreSQL — Project Report



## 1. Project Overview

# Objective

The objective of this project is to build a data pipeline that extracts cafe sales data from a CSV file, inspects and cleans the data, transforms it into an analysis-ready format, and loads it into a PostgreSQL database for SQL-based business analysis.


# Dataset

Source: cafe_sales_dataset.csv

Original records: 10,000

Original columns: 8


# Technology Stack

Python

Pandas

NumPy

PostgreSQL

SQLAlchemy

pgAdmin

Jupyter Notebook


# ETL Workflow

CSV Data → Data Inspection → Data Quality Checks → Data Cleaning → Data Transformation → PostgreSQL Loading → SQL Business Analysis → Professional Report



## 2. Data Quality and Cleaning

### 2.1 Initial Data Quality Assessment

The original dataset, `cafe_sales_dataset.csv`, contained 10,000 records and 8 columns. During inspection, several data quality issues were identified:

- Missing values in multiple columns.
- Invalid placeholders such as `ERROR` and `UNKNOWN`.
- Numeric columns stored as text (`object` data type).
- Transaction dates stored as text rather than datetime values.
- Incomplete transaction details that required careful handling.

### 2.2 Data Cleaning Performed

The following cleaning operations were performed using Python and Pandas:

1. **Handled missing and invalid item values:** Missing values and invalid placeholders in the `Item` column were replaced with `Unknown`.

2. **Standardized categorical columns:** Invalid placeholders in `Payment Method` and `Location` were standardized to `Unknown`, and missing values were filled with `Unknown`.

3. **Converted numeric columns:** `Quantity`, `Price Per Unit`, and `Total Spent` were converted to numeric data types. Invalid numeric entries were converted to missing values using `errors='coerce'`.

4. **Recovered calculable values:** Missing quantities, unit prices, and total amounts were recovered where sufficient valid information was available and the relevant calculation was possible.

5. **Converted transaction dates:** Invalid date placeholders were treated as missing values, and valid dates were converted to datetime format.

6. **Handled data types:** `Quantity` was converted to nullable integer (`Int64`), monetary columns to floating-point values, and categorical text columns to string types.

7. **Validated business rules:** The relationship `Total Spent = Quantity × Price Per Unit` was checked for records where all three values were available. No mismatches were found.

8. **Checked transaction identifiers:** No missing or duplicate transaction IDs were found in the dataset.

### 2.3 Remaining Missing Values

After cleaning, the following missing values remained:

| Column | Missing values |
|---|---:|
| Quantity | 38 |
| Price Per Unit | 38 |
| Total Spent | 40 |
| Transaction Date | 460 |

All other columns had zero missing values after cleaning.

### 2.4 Data Quality Decisions

Some missing numerical values could not be recovered reliably because the required information was unavailable. These values were retained as missing rather than filled with arbitrary numbers.

Missing transaction dates were also retained because a date could not be determined reliably. As a result, these records could not be assigned to a specific month or weekday during time-based analysis.

The value `Unknown` was used for unavailable categorical information. It represents missing or invalid information, not a confirmed business value.

### 2.5 Data Quality Outcome

The cleaning process improved the dataset's consistency, corrected data types, standardized categorical values, and recovered values where possible. The cleaned data was then transformed and loaded into PostgreSQL for business analysis.

Although the dataset was ready for analysis, some missing values remained and were considered when interpreting the SQL results.



## 3. SQL Business Analysis

SQL queries were executed in PostgreSQL using pgAdmin to analyze cafe revenue, transaction patterns, product performance, payment preferences, and location-based sales.

### 3.1 Revenue and Sales Analysis

| Business Metric | Result |
|---|---:|
| Total Revenue | 88,952.00 |
| Average Transaction Value | 8.93 |
| Total Recorded Transactions | 10,000 |
| Total Quantity Sold | 30,141 |

Insights:

- The cafe generated 88,952.00 in recorded revenue.
- The average transaction value was 8.93, calculated using transactions with available revenue.
- The dataset contained 10,000 recorded transactions.
- A total of 30,141 units were counted from available quantity values. Records with missing quantities were excluded from this calculation.

### 3.2 Time-Based Analysis

#### Monthly Sales Performance

| Month | Total Sales |
|---|---:|
| January 2023 | 7,242.00 |
| February 2023 | 6,633.50 |
| March 2023 | 7,214.50 |
| April 2023 | 7,168.00 |
| May 2023 | 6,941.50 |
| June 2023 | 7,350.00 |
| July 2023 | 6,877.50 |
| August 2023 | 7,077.50 |
| September 2023 | 6,846.00 |
| October 2023 | 7,302.00 |
| November 2023 | 6,957.00 |
| December 2023 | 7,177.00 |

Insights:

- June 2023 recorded the highest monthly sales of 7,350.00.
- February 2023 recorded the lowest monthly sales of 6,633.50.
- Monthly sales fluctuated throughout the year rather than showing a consistent upward or downward trend.
- October also performed strongly, with sales of 7,302.00.

These figures represent revenue from records with valid transaction dates and available revenue. Transactions with missing dates could not be assigned to a month.

#### Weekday Performance

| Metric | Result |
|---|---|
| Highest-Sales Weekday | Thursday |
| Total Thursday Sales | 12,394.00 |

Insight: Thursday generated the highest recorded sales among weekdays. The cafe could investigate whether promotions or staffing adjustments on other days might improve performance.

### 3.3 Product Analysis

#### Item Generating the Highest Revenue

| Item | Total Revenue |
|---|---:|
| Salad | 17,320.00 |

Insight: Salad generated the highest recorded revenue among identified items. The cafe could examine its demand, pricing, and ingredient costs before making product or pricing decisions.

#### Item with the Highest Quantity Sold

| Item | Total Quantity Sold |
|---|---:|
| Coffee | 3,534 |

Insight: Coffee had the highest recorded quantity sold among identified items. This indicates strong sales volume, although sales volume alone does not establish profitability.

#### Average Selling Price by Item

| Item | Average Selling Price |
|---|---:|
| Salad | 5.00 |
| Smoothie | 4.00 |
| Sandwich | 4.00 |
| Cake | 3.00 |
| Juice | 3.00 |
| Coffee | 2.00 |
| Tea | 1.50 |
| Cookie | 1.00 |

Insights:

- Salad had the highest average selling price at 5.00 per unit.
- Cookie had the lowest average selling price at 1.00 per unit.
- These results describe selling prices, not product costs or profit margins. Cost data would be needed to evaluate profitability.

Unknown items were excluded from the product comparisons.

### 3.4 Payment Method Analysis

| Payment Method | Transactions |
|---|---:|
| Digital Wallet | 2,291 |

Insight: Digital Wallet was the most frequently used identified payment method. The cafe should continue supporting this option while monitoring preferences for cash and credit cards.

Transactions with unknown payment methods were excluded from this comparison.

### 3.5 Location-Based Sales Analysis

#### Total Sales by Location

| Location | Total Sales |
|---|---:|
| In-store | 27,127.00 |
| Takeaway | 26,487.50 |

Insights:

- In-store sales exceeded Takeaway sales by 639.50.
- The two sales channels had relatively similar recorded revenue.
- The cafe could compare operating costs and customer demand across both channels before making resource-allocation decisions.

Records with unknown locations were excluded from this comparison.

#### Average Transaction Value by Location

| Location | Average Transaction Value |
|---|---:|
| In-store | 9.03 |
| Takeaway | 8.80 |

Insights:

- The average In-store transaction value was 9.03.
- The average Takeaway transaction value was 8.80.
- In-store transactions had an average value 0.23 higher than Takeaway transactions.

The cafe could investigate why the average spending differs between these channels and whether suitable offers could increase Takeaway transaction values.

### 3.6 Overall Business Findings

The SQL analysis identified the following key findings:

1. Total recorded revenue was 88,952.00.
2. June 2023 was the strongest month by recorded revenue.
3. Thursday was the strongest weekday by recorded sales.
4. Salad generated the highest revenue among identified items, while Coffee had the highest quantity sold.
5. Digital Wallet was the most frequently used identified payment method.
6. In-store sales and average transaction value were slightly higher than Takeaway.
7. Some records had missing revenue, dates, quantities, or other information, so the results must be interpreted with these limitations in mind.

The analysis describes observed sales patterns. Further analysis using costs, profit margins, customer details, and operating expenses would be required to make more complete profitability and business-performance decisions.



## 4. Key Business Insights and Recommendations

Based on the SQL analysis, the following insights and recommendations were identified.

### 4.1 Focus on High-Performing Products

Salad generated the highest revenue (17,320.00), while Coffee had the highest quantity sold (3,534 units).

Recommendation: The cafe should monitor the availability and demand for these products. Before prioritizing them for profitability, it should also evaluate ingredient costs, preparation expenses, and profit margins.

### 4.2 Investigate Monthly Sales Patterns

June 2023 recorded the highest monthly sales (7,350.00), while February 2023 recorded the lowest (6,633.50).

Recommendation: The cafe should investigate whether seasonal demand, promotions, customer traffic, or other factors contributed to these differences. Similar successful activities could be considered during slower periods if supported by further analysis.

### 4.3 Review Weekday Performance

Thursday recorded the highest sales, with revenue of 12,394.00.

Recommendation: The cafe could compare staffing, inventory, and promotional activities across weekdays. Staffing and stock levels should reflect observed demand rather than assuming that the same pattern will continue.

### 4.4 Maintain Popular Payment Options

Digital Wallet was the most frequently used identified payment method, with 2,291 transactions.

Recommendation: The cafe should continue supporting digital wallet payments and monitor payment-method trends to ensure customers have convenient payment options.

### 4.5 Evaluate In-Store and Takeaway Performance

In-store generated 27,127.00 in sales, compared with 26,487.50 for Takeaway. The average transaction value was 9.03 for In-store and 8.80 for Takeaway.

Recommendation: The cafe could investigate whether product preferences, order sizes, promotions, or operating costs explain these differences. Takeaway offers could be tested to increase average order value, with results measured before expanding them.

### 4.6 Improve Data Quality

Some records still had missing quantities, prices, revenue, and transaction dates after cleaning.

Recommendation:The cafe should improve data entry and validation procedures, particularly for transaction dates and sales amounts. Better data completeness would support more reliable monthly reporting and business decisions.

### 4.7 Track Profitability, Not Just Revenue

The available dataset contains sales information but does not provide product costs or operating expenses.

Recommendation:The cafe should consider collecting ingredient costs, packaging costs, and other relevant expenses. This would allow future analysis of profit margins and help determine which products are most profitable, rather than relying on revenue alone.

### Summary

The analysis highlights strong performance from Salad and Coffee, identifies June and Thursday as the highest-sales periods, and shows slightly stronger recorded sales and average transaction values for In-store transactions. These findings provide a starting point for business decisions, but recommendations should be tested and reviewed using more complete operational and cost data.



## 5. Limitations and Future Improvements

### 5.1 Data Quality Limitations

The original dataset contained missing values and invalid placeholders. Although cleaning and recovery operations were performed, some values remained missing:

- 38 missing quantities
- 38 missing unit prices
- 40 missing total amounts
- 460 missing transaction dates

These missing values limit some analyses. For example, transactions without valid dates cannot be included in monthly or weekday sales analysis.

### 5.2 Unknown Categorical Values

Missing or invalid values in categorical columns were standardized to `Unknown`. This improved consistency but did not recover the original information.

Records with unknown item names, payment methods, or locations were excluded from the relevant comparisons, so those comparisons do not represent every record in the dataset.

### 5.3 Revenue and Profitability Limitations

The dataset contains sales amounts but does not include ingredient costs, operating expenses, or profit margins. Therefore, the analysis identifies revenue and sales patterns but cannot determine which products are the most profitable.

Future versions could incorporate cost data to support profit analysis and more informed pricing decisions.

### 5.4 Pipeline and Data Loading Limitations

The pipeline was developed using a Jupyter Notebook and PostgreSQL. Data loading and database validation were performed, and the database row count was confirmed as 10,000.

However, the current workflow is primarily manual. Future improvements could include:

- Organizing the ETL logic into reusable Python functions and scripts.
- Adding structured logging and error handling.
- Automating data-quality checks before loading.
- Preventing duplicate records when the pipeline is executed repeatedly.
- Using database constraints to improve data integrity.
- Automating scheduled pipeline execution.

### 5.5 Data Validation and Monitoring

The project includes checks for missing values, data types, transaction identifiers, and the relationship between quantity, unit price, and total amount.

Future versions could add automated validation rules, data-quality summaries, and alerts when unexpected values or loading failures occur.

### 5.6 Future Enhancements

Potential enhancements include:

1. Automated ETL execution: Run extraction, cleaning, transformation, and loading through a reusable Python script.
2. Incremental loading: Load only new or changed records instead of reloading the entire dataset.
3. Data visualization: Build a dashboard using Power BI or another visualization tool.
4. Profitability analysis: Add cost and expense data to calculate profit margins.
5. Pipeline orchestration: Use Apache Airflow to schedule and monitor pipeline execution as the project grows.

These improvements would make the project more automated, maintainable, and suitable for handling larger datasets.

### 5.7 Overall Limitation

This project uses a single cafe sales dataset covering 2023. The findings describe the available records and should not automatically be generalized to other periods or businesses. Further data collection and validation would be required to confirm whether the observed patterns continue over time.



## 6. Conclusion

The Sales ETL Pipeline project demonstrates the process of extracting, cleaning, transforming, and loading cafe sales data using Python, Pandas, and PostgreSQL.

The project began with a CSV dataset containing 10,000 records and 8 columns. Data quality checks identified missing values, invalid placeholders, and incorrect data types. Cleaning and transformation operations improved data consistency, created additional analytical columns, and prepared the dataset for database loading.

The transformed dataset was successfully loaded into PostgreSQL, where the table was validated to contain 10,000 records. SQL queries were then used to analyze revenue, transaction values, monthly and weekday sales patterns, product performance, payment preferences, and location-based sales.

The analysis identified Salad as the highest-revenue item, Coffee as the highest-quantity item, June 2023 as the highest-sales month, and Thursday as the highest-sales weekday. In-store transactions also showed slightly higher total sales and average transaction value than Takeaway transactions.

The project also highlighted the importance of data quality. Some missing values remained after cleaning, and these limitations were considered when interpreting the results.

Overall, this project provided practical experience with Python-based data processing, ETL concepts, PostgreSQL database loading, SQL business analysis, and data-quality validation. It establishes a foundation for future improvements, including automated ETL execution, incremental loading, pipeline monitoring, and business intelligence dashboards.


