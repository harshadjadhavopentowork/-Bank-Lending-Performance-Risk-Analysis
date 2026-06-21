# Bank Loan Risk & Performance Analytics Dashboard

## Project Overview

This repository contains a comprehensive, end-to-end data analytics project focused on monitoring and assessing bank lending activities. The core objective is to deliver robust, data-driven insights into key loan-related performance indicators (KPIs), track portfolio health, evaluate regional/borrower risk profiles, and identify temporal trends to optimize underwriting rules and credit risk strategies.

The project maps complex financial data across three distinct dashboard structures, facilitating seamless navigation from executive-level operational metrics to granular portfolio details.

---

## Technical Stack & Assets

* **Database Management:** SQL (PostgreSQL / SQL Server) for complex relational queries and backend verification.


* **Data Manipulation:** Python (Pandas, NumPy) for advanced ETL, structured schema building, and preprocessing.
* **Business Intelligence Tool:** Tableau / Power BI for relational data modeling and presentation.


* **Project Assets:**
* `financial_loan_2.csv`: The primary raw tabular dataset containing individual transactional records across 3,900+ entries.


* `financial_loan_data_excel_2.xlsx`: Preprocessed, multi-sheet analytical workbook summarizing credit risk distributions.


* `Tableau Background 1_2.jpg`, `Tableau Background 2_2.jpg`, `Tableau Background 3_2.jpg`: Custom-designed user interface layouts providing clear visual hierarchy across all reporting tabs.





---

## Database Architecture & Query Schema

All backend verification is built upon a standard relational data schema. The full analytical script is preserved in the project query documentation. Key operations include:

### 1. Portfolio Health Calculations (Good vs. Bad Loans)

Loans with a status of `Fully Paid` or `Current` are classified as high-performing assets ("Good Loans"), while accounts labeled as `Charged Off` denote systemic credit defaults ("Bad Loans").

```sql
-- Good Loan Percentage Distribution
SELECT
    (COUNT(CASE WHEN loan_status = 'Fully Paid' OR loan_status = 'Current' THEN id END) * 100.0) / 
    COUNT(id) AS Good_Loan_Percentage
FROM bank_loan_data;

-- Bad Loan Application Volume
SELECT COUNT(id) AS Bad_Loan_Applications 
FROM bank_loan_data
WHERE loan_status = 'Charged Off';

```

### 2. Temporal & Strategic Breakdowns

```sql
-- Monthly Operational Trends
SELECT 
    MONTH(issue_date) AS Month_Number, 
    DATENAME(MONTH, issue_date) AS Month_Name, 
    COUNT(id) AS Total_Loan_Applications,
    SUM(loan_amount) AS Total_Funded_Amount
FROM bank_loan_data
GROUP BY MONTH(issue_date), DATENAME(MONTH, issue_date)
ORDER BY Month_Number;

```

---

## Dashboard Architecture

### Dashboard 1: Executive Summary

A high-level cockpit designed for C-suite risk officers to monitor lending volumes, capital distributions, and liquidity collection frameworks:

* **Core Metrics:** Tracks Total Loan Applications (38,576 records), Total Funded Amount ($435.75M), and Total Cash Collected ($473.07M) alongside Month-to-Date (MTD) and Month-over-Month (MoM) indicators.


* **Credit Quality Ratios:** Monitors financial health through Average Interest Rate (12.05%) and Average Debt-to-Income (DTI) Ratio (13.33%).


* **Portfolio Status Grid:** Groups data dynamically by operational status to isolate capitalization differences across active vs. defaulted loan accounts.



### Dashboard 2: Strategic Overview

Focuses on identifying demographic trends, structural variations, and geographic correlations within the lending pipeline:

* **Monthly Seasonality (Line Chart):** Illustrates fluctuations in systemic credit demand over a 12-month calendar horizon.


* **Geographic Vulnerability (Filled Map):** Displays regional concentration across address states to locate regional economic variances.


* **Structural Term Split (Donut Chart):** Segregates risk profiles between short-term commitments (36 months) and long-term capital dependencies (60 months).


* **Risk Gating Indicators:** Breaks down total volume against Employee Length (Bar Chart), Loan Purpose (Bar Chart), and Home Ownership (Tree Map) to isolate compounding risk variables.



### Dashboard 3: Granular Details

Serves as a unified single-point interface allowing risk managers to perform deep-dive audits on individual borrower profiles. It integrates multi-dimensional filter layers (e.g., Credit Grade, Employment Sub-grade, Verification Status) to analyze credit risk parameters dynamically.

---

## Strategic Findings & Business Impact

1. **High Asset Baseline Stability:** 86.18% of the loan applications reflect stable repayment activity ("Good Loans"), creating a highly reliable cash flow pipeline for the organization.


2. **Concentration Risk Exposure:** Debt consolidation stands out as the primary reason for financing requests across all demographic groups.


3. **Risk Gating Recommendation:** To limit potential default contagion within the remaining 13.82% "Bad Loan" segment, credit policies must institute strict debt-to-income (DTI) caps specifically targeted at high-volume consolidation requests.
