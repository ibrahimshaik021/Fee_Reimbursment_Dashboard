# End-to-End Employee Reimbursement Analysis in Power BI

## Objective
This project demonstrates a complete data analysis workflow using Microsoft Power BI. The goal was to take a raw employee reimbursement dataset, perform necessary cleaning and transformations, create robust calculations with DAX, and build an interactive dashboard to visualize key performance indicators and extract actionable insights.

---

## Tools Used
- **Power BI** (Power Query for ETL, DAX for calculations, Report View for visualization)

---

## Process

### 1. Data Cleaning and Transformation (Power Query)
The initial dataset contained several data quality issues that required cleaning before any analysis could be performed.

**Problem:** The `Expense_type` and `Project_Name` columns had numerous inconsistencies, including spelling errors, inconsistent casing (e.g., "TRANsporTAAtion"), leading/trailing spaces, and punctuation errors (e.g., ".Miscellaneous").

**Before Cleaning (Screenshot of messy data):**
<img width="1443" height="590" alt="Raw Dataset" src="https://github.com/user-attachments/assets/3f7a5046-c2b3-4f22-b71c-4bb36a8d342c" />




**Solution:**
To standardize the data, the following transformations were applied using the Power Query Editor:
- **Trimmed Whitespace:** Removed all leading and trailing spaces from text columns.
- **Standardized Casing:** Used "Capitalize Each Word" to ensure consistent formatting.
- **Replaced Values:** Corrected specific misspellings and removed extraneous punctuation to consolidate categories.

### 2. Calculated Columns (Power Query)
Two new columns were engineered to enrich the dataset:
- **`Currency_name`:** A new column was created to handle missing currency values. It used conditional logic (`if/then/else`) to assign "INR" or "USD" based on the transaction amount if the original currency field was null.
- **`Amount_INR`:** A final, unified amount column was created by converting all USD and EURO transactions to INR, using assumed exchange rates (1 USD = 83 INR, 1 EURO = 90 INR). This step was crucial for accurate financial aggregation.

### 3. DAX Measures for Analysis
After loading the clean data into the model, three key DAX measures were created to power the visuals:

1.  **Total Reimbursed Amount:** A base measure to sum all transactions.
    - `Total_reimbursed_amount = SUM(fact_reimbursement[Amount_INR])`

2.  **Amount for a Specific Project:** A measure to calculate the total reimbursement amount filtered for "Project_B" only, demonstrating the use of `CALCULATE`.
    - `Amount_reimbursment_Project_B = CALCULATE([Total_reimbursed_amount], fact_reimbursement[Project_Name] = "Project_B")`

3.  **Count of Declined Requests:** A measure to count specific events in the dataset.
    - `Decline_Count = CALCULATE(COUNTROWS(fact_reimbursement), fact_reimbursement[Approval_Status] = "Declined")`

---

## Final Dashboard & Key Insights

The final interactive dashboard provides a clear overview of the reimbursement data.
<img width="1203" height="678" alt="Power_BI_Dashboard" src="https://github.com/user-attachments/assets/4857a40f-16e2-487b-a2fb-ae7d58979fb0" />


### Key Insights Uncovered:
- **Project Dominance:** Project A is the most significant contributor to costs, accounting for **54.78%** of the total reimbursed amount (333K INR).
- **Top Claimant:** The employee "Siva" has the highest total reimbursement amount, exceeding 130K INR.
- **Transaction Volume:** The model processed 100 reimbursement requests, of which only 2 were declined.
