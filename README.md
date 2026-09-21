# 🏦 Bank Loan Portfolio & Risk Analysis Dashboard

A CV-ready Power BI project modeled on real-world loan portfolio reporting used in banks and lending companies. It answers key business questions — how much has been lent, how much is coming back, which loans are risky, and where the bank should focus — using a full **Power Query → Data Model → DAX** workflow.

---

## 📌 Project Overview

**Title:** Bank Loan Portfolio & Risk Analysis Dashboard

**Scenario:** A lending company wants to understand its loan book — total money disbursed, how much has been recovered, which loans are "good" (paying on time) vs "bad" (charged off / defaulted), and how risk varies by grade, region, and purpose.

**Business Questions Answered:**
- How much have we lent out, and how much have we actually received back?
- What percentage of our loan book is "good" vs "bad"?
- Which loan grade, purpose, or region carries the most risk?
- How has monthly loan issuance trended over the last few years?
- Who are our highest-risk customer segments (by income, employment, DTI)?

---

## 🗂️ Dataset Structure

The project uses 3 relational CSV files forming a star-schema-ready dataset:

| File | Type | Key Columns |
|------|------|-------------|
| Customers.csv | Dimension | MemberID, CustomerName, EmpTitle, EmpLength, HomeOwnership, VerificationStatus, AnnualIncome, State |
| StateRegion.csv | Dimension | State, Region |
| Loans.csv | Fact | LoanID, MemberID, LoanAmount, FundedAmount, Term, IntRate, Installment, Grade, SubGrade, IssueDate, LoanStatus, Purpose, ApplicationType, DTI, TotalPayment, LastPaymentDate |

The raw data intentionally included real-world data quality issues — duplicate rows, inconsistent text casing, mixed date formats, missing values, and stray negative DTI entries — all cleaned in Power Query.

---

## 🛠️ Tools & Techniques

- Power BI Desktop
- Power Query (M) — data cleaning & transformation
- Data Modeling — star schema with a dedicated Date table
- DAX — 11+ measures including time-intelligence functions

---

## 🔧 What Was Done

### 1. Data Cleaning (Power Query)
- Removed duplicate rows (MemberID, LoanID)
- Standardized text casing (Trim + Capitalize Each Word)
- Handled missing values (median income, average interest rate by grade, "Not Specified" for blank purpose)
- Parsed inconsistent date formats
- Cleaned invalid negative DTI entries

### 2. Data Transformation
- Merged Loans, Customers, and StateRegion tables
- Created a LoanCategory calculated column (Good Loan / Bad Loan) based on loan status
- Created a RiskBand calculated column (Low / Medium / High) based on DTI thresholds

### 3. Data Modeling (Star Schema)
- Loans (fact) related to Customers (dimension, via MemberID) related to StateRegion (dimension, via State)
- Dedicated DateTable built with CALENDAR(), marked as the official Date Table, and related to Loans[IssueDate]

### 4. DAX Measures
| Measure | Purpose |
|---|---|
| Total Applications | COUNTROWS(Loans) |
| Total Funded Amount | SUM(Loans[FundedAmount]) |
| Total Received | SUM(Loans[TotalPayment]) |
| Avg Interest Rate | AVERAGE(Loans[IntRate]) |
| Avg DTI | AVERAGE(Loans[DTI]) |
| Good Loan % | Good loans as % of total applications |
| Bad Loan % | Bad loans as % of total applications |
| Recovery Rate % | Total Received divided by Total Funded Amount |
| MTD Funded Amount | TOTALMTD() time-intelligence measure |
| MoM Growth % | Month-over-month funded amount growth |
| High Risk Amount | Funded amount filtered to High risk band |

---

## 📊 Dashboard Pages

**1. Summary Page** — KPI cards (Total Applications, Total Funded Amount, Total Received, Good Loan %, Bad Loan %), funded amount by grade, and a Good vs Bad Loan breakdown, with Region and Grade slicers.

**2. Trend Page** — Monthly funded amount trend, split by Good Loan / Bad Loan, showing loan issuance patterns over time.

**3. Risk Page** — Bad Loan % broken down by Grade, Purpose, and Region, plus a DTI vs Recovery Rate scatter analysis to identify high-risk segments.

**4. Customer Page** — Top borrower table, Income vs Loan Amount scatter plot, and a HomeOwnership breakdown of applicants.

---

## 📈 Key Findings

- Analyzed 900+ loan records across a multi-year issuance period
- Overall Good Loan rate of approximately 77%, with a Bad Loan (charged off/late) rate of approximately 23%
- Bad loan concentration is highest in specific grades and loan purposes, flagging them as priority areas for underwriting review
- Clear positive relationship observed between applicant income and loan amount

---

## 📁 Files in This Repo

- Bank_Loan_Portfolio_Analysis.pbix — the full Power BI file
- README.md — this file

---

## 🚀 How to Use

1. Download Bank_Loan_Portfolio_Analysis.pbix
2. Open in Power BI Desktop (free download from Microsoft)
3. Explore the Summary, Trend, Risk, and Customer pages using the slicers

---

## 👤 Author

Built as a self-guided portfolio project to demonstrate the full BI workflow: messy raw data → cleaned → modeled → DAX KPIs → interactive dashboard.
