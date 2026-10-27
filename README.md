# IKEA Gifting: Customer Analytics and RFM Segmentation
### An End-to-End Data Analytics Project Using SQL, Python, and Power BI

**Author:** Arena Patra | Data Analyst

---

## Disclaimer

IKEA Gifting is a fictional company created for this educational portfolio project. It is not affiliated with, endorsed by, or representative of the real IKEA corporation. All financial figures, customer counts, and business metrics are derived from a public, anonymized transactional dataset and do not reflect the actual performance of any real company. IKEA-inspired visual branding is used solely for dashboard design and storytelling purposes.

---

## Project Overview

This project simulates a real-world analytics engagement for a fictional UK-based gifting and home decor e-commerce brand, IKEA Gifting.

The objective was to transform raw, messy transactional data into a decision-ready business intelligence dashboard using an end-to-end data analytics workflow.

The project combines **SQL Server, Python, and Power BI** to clean and validate transaction data, perform customer segmentation using Recency, Frequency, and Monetary (RFM) analysis, identify customers at risk of churn, and communicate actionable business insights through an interactive dashboard.

The project follows a business-first approach: defining the business problem before exploring the data and ensuring that every technical step supports a specific business question.

---

## Business Problem

The business wants to identify its most valuable customers and reduce churn among customers who may be at risk of leaving.

Currently, leadership lacks a structured understanding of customer value and purchasing behaviour. As a result, customer retention efforts are reactive rather than targeted.

The analysis aims to help the business understand customer value, identify high-risk customers, and develop segment-specific retention strategies.

---

## Business Requirements

The project focuses on answering five key business questions:

1. Who are the most valuable customers, and how much revenue do they contribute?
2. Which customers have become inactive and may be at risk of churning?
3. How do customer value and churn trends change across months, quarters, and years?
4. How does customer value and churn differ across geographic markets?
5. What targeted business actions should be taken for each customer segment?

---

## Key Findings

- **Revenue Concentration:** Champions represent approximately 22% of the customer base but generate close to 68% of total revenue, highlighting a strong concentration of revenue among high-value customers.
- **Customer Churn:** Overall churn is approximately 50.86%, based on a 90-day inactivity threshold, indicating a significant customer retention challenge.
- **Customer Value:** Average customer value varies considerably across segments, ranging from a few hundred in the Lost segment to several thousand among Champions.
- **Seasonal Trends:** Revenue shows clear seasonal spikes in Q4, consistent with the purchasing patterns of a gifting-focused business.
- **Retention Opportunities:** RFM segmentation enables the business to prioritize valuable customers, re-engage inactive customers, and develop targeted retention campaigns.

*Note: These findings are based on the project's analysis of the public Online Retail II dataset and represent the fictional IKEA Gifting business scenario.*

---

## Data Source

**Online Retail II — UCI Machine Learning Repository**

The dataset was donated by Daqing Chen and contains anonymized transactional records from a UK-based, non-store online retailer selling a variety of occasion giftware.

| Dataset Attribute | Details |
|---|---|
| Source | UCI Machine Learning Repository |
| Dataset | Online Retail II |
| Time Period | December 2009 – December 2011 |
| Raw Transaction Rows | 1,067,371 |
| Original Sheets | Two |
| Data Type | Anonymized retail transaction data |
| License | Creative Commons Attribution 4.0 International |

The dataset contains real-world data quality challenges, including mixed date formats, duplicate transactions, invalid transaction records, and missing customer identifiers.

These issues make it suitable for demonstrating practical data cleaning and preparation techniques.

---

## Tools and Tech Stack

| Tool | Purpose in This Project |
|---|---|
| SQL Server / SSMS | Data import, cleaning, deduplication, type conversion, and exploratory querying across the 1M+ row dataset |
| Python (pandas) | RFM calculation, customer scoring, segmentation, and churn flagging |
| Jupyter Notebook | Interactive environment for Python-based analysis |
| Power BI Desktop | Development of a three-page interactive business intelligence dashboard |
| DAX | Measures for customer counts, revenue, average customer value, and other KPIs |

---

## Core Concepts Demonstrated

- **RFM Analysis:** Scoring customers based on Recency, Frequency, and Monetary value to measure customer value and purchasing behaviour.
- **Customer Segmentation:** Grouping customers into meaningful business segments, including Champions, Loyal, At Risk, New, Needs Attention, and Lost.
- **Churn Definition and Flagging:** Applying a fixed 90-day inactivity threshold to identify customers who may have stopped purchasing.
- **Data Quality Handling:** Resolving mixed date formats, converting data types, handling invalid records, and detecting duplicate transactions using SQL window functions.
- **Dimensional Modeling for BI:** Preparing a unified analysis-ready dataset to support consistent reporting across multiple dashboard pages.
- **Interactive BI Design:** Implementing slicers, bookmark-based toggles, KPI cards, trend analysis, and multi-page report navigation.

---

## Project Workflow

### Step 1 — Business Requirements Document

Defined the business problem, analytical scope, key business questions, metric definitions, and success criteria before beginning data processing.

### Step 2 — SQL: Data Cleaning and Exploration

- Imported both raw dataset sheets into SQL Server.
- Corrected data types and handled inconsistent date formats.
- Resolved a mixed date-format issue affecting a substantial portion of the dataset.
- Identified and removed 32,907 exact duplicate rows.
- Validated row counts throughout the cleaning process against the published UCI dataset totals.
- Performed exploratory queries to assess data quality and understand transaction patterns.

### Step 3 — Python: RFM Analysis and Customer Segmentation

- Loaded the cleaned dataset into pandas.
- Aggregated transaction-level data to calculate customer-level Recency, Frequency, and Monetary values.
- Assigned each customer an RFM score from 1 to 5 for each dimension.
- Classified 5,881 customers into six business-oriented segments.
- Created a 90-day inactivity flag to identify customers classified as churned under the project's definition.

### Step 4 — Power BI: Dashboard Development

- Integrated the customer segmentation results with transaction-level data.
- Created a unified analysis-ready table for reporting.
- Developed a three-page interactive Power BI report.
- Built KPI cards, customer segment breakdowns, revenue trends, and churn analysis.
- Added slicers, bookmark-based toggles, and page navigation to support interactive exploration.

---

## Dashboard Structure

### Page 1 — Home

The landing page introduces the project and provides navigation to the two analytical report pages.

**Key components:**
- Project title and introduction.
- Navigation buttons to Business Overview and RFM Analysis.
- Glossary explaining Recency, Frequency, Monetary, and Churned %.

### Page 2 — Business Overview

Provides an executive summary of overall business performance and customer behaviour.

**Key Performance Indicators (KPIs):**
- Total Customers
- Total Revenue
- Churn %
- Average Customer Value

**Visualizations and Features:**
- Revenue and customer-count toggle.
- Customer segment breakdowns.
- Monthly, quarterly, and yearly trend analysis.
- Interactive filters for month, quarter, year, country, customer segment, and churn status.

### Page 3 — RFM Analysis

Provides a detailed view of customer segment performance and retention patterns.

**Key components:**
- Segment performance summary table.
- Customer count and revenue by segment.
- Average Recency, Frequency, and Monetary scores by segment.
- Average customer value by segment.
- Churn percentage trends over time.

---

## Sample SQL: Handling Mixed Date Formats

The following SQL query demonstrates how to handle multiple date formats using `TRY_CONVERT()` and `COALESCE()`.

```sql
SELECT
    Invoice,
    StockCode,
    Description,
    Quantity,
    COALESCE(
        TRY_CONVERT(datetime2, InvoiceDate, 120),
        TRY_CONVERT(datetime2, InvoiceDate, 105)
    ) AS InvoiceDate,
    TRY_CONVERT(decimal(10,2), Price) AS Price,
    Customer_ID,
    Country
INTO dbo.RetailCombined
FROM dbo.[Year 2009-2010];
```

**Explanation:**

- `TRY_CONVERT()` attempts to convert a value into the specified data type without raising a conversion error for invalid values.
- Style `120` handles the `yyyy-mm-dd hh:mi:ss` date format.
- Style `105` handles the `dd-mm-yyyy` date format.
- `COALESCE()` returns the first successfully converted, non-NULL date.
- `TRY_CONVERT(decimal(10,2), Price)` converts the price into a decimal data type suitable for monetary calculations.
- `SELECT INTO` creates a new table named `RetailCombined` containing the selected columns and converted values.

*Note: This is an illustrative query for the first source sheet. The complete cleaning workflow should process both sheets and validate conversions, invalid records, and duplicate handling.*

---

## Sample Python: RFM Scoring

The following Python code assigns scores from 1 to 5 to customers across the three RFM dimensions.

```python
import pandas as pd

# Recency: Lower recency means a more recent purchase.
rfm['R_Score'] = pd.qcut(
    rfm['Recency'],
    5,
    labels=[5, 4, 3, 2, 1]
).astype(int)

# Frequency: Higher purchase frequency receives a higher score.
rfm['F_Score'] = pd.qcut(
    rfm['Frequency'].rank(method='first'),
    5,
    labels=[1, 2, 3, 4, 5]
).astype(int)

# Monetary: Higher spending receives a higher score.
rfm['M_Score'] = pd.qcut(
    rfm['Monetary'],
    5,
    labels=[1, 2, 3, 4, 5]
).astype(int)
```

**Explanation:**

- **Recency:** Customers who purchased more recently receive higher scores.
- **Frequency:** Customers who purchase more frequently receive higher scores.
- **Monetary:** Customers who generate more revenue receive higher scores.
- `pd.qcut()` divides values into five quantile-based groups.
- `rank(method='first')` helps handle duplicate Frequency values when creating quantile groups.

The resulting RFM scores are used to classify customers into business segments.

*Note: The scoring logic may require adjustments if the data contains too few distinct values or if quantile boundaries cannot be calculated.*

---

## Business Recommendations

Based on the analytical findings, the following actions are recommended.

| Customer Segment | Recommended Business Action |
|---|---|
| Champions | Reward high-value customers with exclusive benefits, early access, and loyalty rewards. |
| Loyal | Encourage repeat purchases through loyalty programmes and personalized offers. |
| At Risk | Launch targeted re-engagement campaigns and personalized incentives. |
| New | Introduce onboarding campaigns and recommendations to encourage a second purchase. |
| Needs Attention | Use personalized product recommendations and limited-time offers to stimulate repeat purchases. |
| Lost | Test win-back campaigns and targeted discounts to assess whether inactive customers can be reactivated. |

These recommendations help the business allocate marketing resources more effectively by matching retention strategies to customer behaviour and value.

---

## Key Business Impact

This project demonstrates how an end-to-end data analytics workflow can support customer-centric business decisions.

- **Improved Customer Understanding:** RFM scoring provides a structured view of customer purchasing behaviour and value.
- **Revenue Protection:** Identifying high-value customers helps the business prioritize retention efforts.
- **Targeted Marketing:** Segment-based recommendations support more relevant and personalized campaigns.
- **Churn Monitoring:** A consistent inactivity threshold makes it possible to track customer inactivity over time.
- **Data-Driven Decision-Making:** Interactive Power BI dashboards help stakeholders explore trends and identify opportunities.

The project establishes a foundation for testing targeted retention strategies and measuring their effectiveness over time.

---

## How to Reproduce This Project

Follow these steps to recreate the analysis.

### Prerequisites

Install or prepare the following tools:

- Microsoft SQL Server and SQL Server Management Studio (SSMS).
- Python with pandas and the other libraries required by the notebook.
- Jupyter Notebook.
- Microsoft Power BI Desktop.

### Step 1 — Download the Dataset

Download the Online Retail II dataset from the UCI Machine Learning Repository.

### Step 2 — Run the SQL Scripts

- Open the scripts in the `01_sql` folder.
- Execute the scripts in the documented order.
- Recreate the cleaned `RetailCleaned` table.
- Verify row counts, data types, duplicate handling, and date conversions.

### Step 3 — Run the Python Notebook

- Export the cleaned `RetailCleaned` table to CSV.
- Open the notebook in the `02_python` folder.
- Update the input file path.
- Run the notebook to calculate RFM metrics, generate customer scores, assign segments, and create the churn flag.
- Export the resulting segmented dataset.

### Step 4 — Refresh the Power BI Dashboard

- Open the Power BI file in the `03_powerbi` folder.
- Update the data source path to point to the exported dataset.
- Refresh the data.
- Verify the KPIs, customer segments, trend charts, slicers, and navigation.

**Important:** File and folder names may differ depending on how the project repository is organized. Update file paths and connection settings accordingly.

---

## Project Structure

```text
IKEA-Gifting-Customer-Analytics/
│
├── README.md
│
├── 01_sql/
│   ├── data_import.sql
│   ├── data_cleaning.sql
│   ├── deduplication.sql
│   └── exploratory_analysis.sql
│
├── 02_python/
│   └── rfm_segmentation.ipynb
│
├── 03_powerbi/
│   └── ikea_gifting_dashboard.pbix
│
└── data/
    ├── RetailCleaned.csv
    └── customer_segments.csv
```

*The structure above is a suggested organization. Adjust the filenames and folders to match the actual contents of your repository.*

---

## Conclusion

The IKEA Gifting Customer Analytics project demonstrates a complete data analytics workflow, from defining business requirements and cleaning raw transactional data to building customer segments and communicating insights through interactive dashboards.

By combining SQL Server, Python, and Power BI, the project showcases practical skills in data preparation, customer analytics, RFM segmentation, churn identification, and business intelligence reporting.

More importantly, it connects technical analysis to business decisions by identifying valuable customer groups, highlighting retention risks, and recommending targeted actions.

**Skills Demonstrated:** SQL | Data Cleaning | Python | pandas | RFM Analysis | Customer Segmentation | Churn Analysis | Power BI | DAX | Data Visualization | Business Intelligence
