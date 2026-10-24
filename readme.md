# 🛒 Executive Sales & Customer Analytics Dashboard (UK E-Commerce)
### An End-to-End Power BI Business Intelligence & UI/UX Case Study

![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-00599C?style=for-the-badge&logo=cplusplus&logoColor=white)
![Power Query](https://img.shields.io/badge/Power_Query-00838F?style=for-the-badge&logo=excel&logoColor=white)
![Data Modeling](https://img.shields.io/badge/Data_Modeling-Star_Schema-blue?style=for-the-badge)

---

## 📌 1. Executive Summary

This case study showcases an end-to-end Business Intelligence solution built in Power BI to analyze transaction logs from a UK-based online retailer (~500,000 records spanning Dec 2010 – Dec 2011). 

The goal was to transform an unstructured, flat transactional file into an executive-grade analytical platform. The final solution features a normalized **Star Schema** data model, robust **Time-Intelligence DAX calculations**, and a **2-Page interactive SaaS-style dashboard** engineered using modern UI/UX principles.

### Key Impact Metrics Identified
* **Total Net Revenue:** £8.91M across 19K orders and 4K unique customers.
* **Geographic Focus:** The UK accounts for **>80%** of total revenue (£8.91M aggregate), with Netherlands, EIRE, and Germany leading international expansion.
* **Peak Seasonality:** November 2011 represented the highest monthly revenue (£1.16M), representing a **+11.8% MoM increase** driven by holiday shopping surges.
* **Customer Concentration:** Top 1% of customers generate disproportionate revenue, led by Customer ID `14646` (£280K+ lifetime sales).

---

## 🎯 2. Business Problem Statement & Objectives

### The Business Challenge
The executive leadership team of the e-commerce store faced several critical reporting challenges:
1. **Inaccurate Revenue Tracking:** Raw transactional data included returned and canceled orders (negative quantities) mixed with gross sales, creating distorted top-line figures.
2. **Lack of MoM Insights:** Stakeholders could only view historical static totals rather than dynamic Month-over-Month (MoM) growth percentages or prior-period comparisons.
3. **Unclear Customer Segmentation:** Leadership lacked clear visibility into VIP account spending, making account management and retention strategies difficult to target.
4. **Poor Report Aesthetics:** Legacy reports were dense, unformatted, and difficult for C-suite executives to digest quickly.

### Strategic Objectives
* **Data ETL:** Clean, filter, and structure raw transactional data in Power Query without losing aggregate sales integrity.
* **Architecture:** Normalize the flat table into a 1-to-Many Star Schema with a dedicated Date dimension.
* **Analytics:** Build dynamic DAX measures for Net Revenue, Average Order Value (AOV), MoM Revenue %, and Time-Intelligence calculations.
* **UI/UX Design:** Design a clean, 2-page executive presentation using SaaS container design principles (`#F1F5F9` canvas, `#FFFFFF` cards, 8px rounded borders, left accent strips).

---

## 🏗️ 3. Data Architecture & ETL Pipeline

### Raw Dataset Overview
* **Source:** UCI Machine Learning / Kaggle Online Retail Dataset
* **Volume:** ~540,000 rows
* **Key Fields:** `InvoiceNo`, `StockCode`, `Description`, `Quantity`, `InvoiceDate`, `UnitPrice`, `CustomerID`, `Country`

### Power Query ETL Pipeline
During the ETL phase in Power Query (M), several critical data transformation steps were executed:

1. **Date Standardisation (Fixing Date Key Mismatches):**
   * *Problem:* `InvoiceDate` contained timestamps (e.g., `12/1/2010 8:26 AM`), preventing direct relationships with standard daily date dimensions (`12/1/2010 12:00 AM`).
   * *Solution:* Explicitly converted `InvoiceDate` to a pure `Date` data type in Power Query, stripping time signatures to ensure exact key matching with `Dim_Date`.

2. **Handling Missing Customer Keys:**
   * *Problem:* Approximately ~135k rows lacked a `CustomerID`.
   * *Solution:* Preserved sales rows to maintain accurate revenue aggregates while assigning unmapped customer records to a standardized `Unknown Customer` entity.

3. **Returns and Cancellations Logic:**
   * *Problem:* Invoices starting with "C" represented cancellations with negative quantities.
   * *Solution:* Maintained negative values in the fact table so that `SUMX` calculations naturally calculate true **Net Revenue** rather than Gross Sales.

### Star Schema Architecture

```text
                  +-------------------+
                  |     Dim_Date      |
                  +-------------------+
                            | 1
                            |
                            | *
+-------------------+     +-------------------+     +-------------------+
|   Dim_Customer    |1   *|    Fact_Sales     |*   1|    Dim_Product    |
+-------------------+-----|-------------------|-----+-------------------+
                          | (InvoiceNo,       |
                          |  Quantity,        |
                          |  UnitPrice,       |
                          |  InvoiceDate,     |
                          |  CustomerID,      |
                          |  StockCode)       |
```

* **`Fact_Sales`:** Contains line-item transaction metrics (`Quantity`, `UnitPrice`).
* **`Dim_Customer`:** Contains unique `CustomerID` and primary geographical location (`Country`).
* **`Dim_Product`:** Contains unique `StockCode` and standardized `Description`.
* **`Dim_Date`:** A custom calendar dimension containing `Date`, `Year`, `Month Name`, `Year-Month`, `Quarter`, and `Month Number`.

---

## 📐 4. Data Modeling & DAX Measures

All calculations were centralized into a dedicated `_Key Measures` table to ensure scalar consistency across all visual containers.

### Core Metrics

#### 1. Net Revenue
```dax
Net Revenue = 
SUMX(
    Fact_Sales,
    Fact_Sales[Quantity] * Fact_Sales[UnitPrice]
)
```

#### 2. Total Orders
```dax
Total Orders = 
DISTINCTCOUNT(Fact_Sales[InvoiceNo])
```

#### 3. Total Customers
```dax
Total Customers = 
DISTINCTCOUNT(Fact_Sales[CustomerID])
```

#### 4. Average Order Value (AOV)
```dax
Average Order Value = 
DIVIDE(
    [Net Revenue],
    [Total Orders],
    0
)
```

### Time Intelligence Metrics

#### 5. Previous Month Revenue
```dax
Previous Month Revenue = 
CALCULATE(
    [Net Revenue],
    DATEADD(Dim_Date[Date], -1, MONTH)
)
```

#### 6. Month-over-Month (MoM) Growth %
```dax
MoM Growth % = 
DIVIDE(
    [Net Revenue] - [Previous Month Revenue],
    [Previous Month Revenue],
    0
)
```

---

## 🎨 5. UI/UX Design System & Dashboard Layout

To elevate this report to executive portfolio quality, a SaaS-inspired UI/UX container framework was implemented.

### Color Palette & Design Rules
* **Canvas Background:** `#F1F5F9` (Light Slate Grey) — Eliminates harsh white glare and provides visual structure.
* **Card Containers:** `#FFFFFF` (Pure White) with an **8px corner radius**, a subtle **1px `#E2E8F0` light slate border**, and a soft outer drop shadow (`10px blur, 92% transparency`).
* **Typography Hierarchy:** Bold `#0F172A` (Dark Slate) for core numeric callouts (`28pt`); muted `#64748B` (Grey) for category labels (`10pt`).
* **Brand Accents:** `#2563EB` (Royal Blue) for primary metrics and accent strips; `#1E3A8A` (Navy Blue) for top rank highlighting.

---

### Dashboard Structure

#### **Page 1: Executive Sales Overview**
Targeted at C-suite stakeholders requiring high-level macro visibility.
* **KPI Header Row:** 4 Card containers (`Net Revenue`, `Average Order Value`, `Total Orders`, `Total Customers`) featuring a thin 4px blue accent border on the left edge.
* **Monthly Net Revenue Trend (Line Chart):** Smooth line visual with area gradient shading below the line and gridline decluttering.
* **Top 10 Products (Horizontal Bar Chart):** Dynamic `Top N` dynamic filter ranking top revenue generators.
* **Geographic Market Distribution:** Bar chart highlighting UK market dominance alongside secondary European markets.
* **Interactive Slicers:** Custom tile-styled horizontal pill buttons for `Year` selection and a sleek `Country` dropdown.

#### **Page 2: Product & Customer Deep Dive**
Targeted at inventory managers and VIP account leads requiring granular detail.
* **Top High-Value Customer Matrix:** Ranks top spenders (e.g., Customer `14646` at £280K+) with drill-down metrics for order count and AOV.
* **Price vs. Volume Scatter Plot:** Analyzes product elasticity by plotting `Average Unit Price` (X-axis) against `Quantity Sold` (Y-axis), with bubble size mapped to `Net Revenue`.
* **Product Detail Table:** Full inventory performance table featuring alternating `#FFFFFF` and `#F8FAFC` zebra striping for high readability.

---

## 💡 6. Key Business Findings & Strategic Insights

### 1. Geographic Revenue Concentration
* **Finding:** The United Kingdom dominates sales, generating **over 80%** of total revenue.
* **Secondary Markets:** Netherlands (£280K+), EIRE (£250K+), and Germany (£220K+) represent the highest-performing international expansion channels.

### 2. Strong Q4 Seasonality Peak
* **Finding:** Net revenue remains relatively stable from January through August (~£450K–£650K/month) before experiencing a massive uptick in Q4:
  * **September 2011:** £952K (+47.6% MoM)
  * **October 2011:** £1.03M (+9.1% MoM)
  * **November 2011:** £1.16M (+11.8% MoM — Year Peak)

### 3. Customer Pareto Distribution (80/20 Rule)
* **Finding:** Revenue is heavily concentrated among a small percentage of B2B/wholesale accounts.
* **Example:** Customer `14646` (Netherlands) generated **£280,206** across 141 orders, whereas the average customer spend is **~£2,000**.

### 4. Product Elasticity Insights
* **Finding:** High-volume items are concentrated in lower price brackets (£1.00 – £5.00).
* **Top Product:** `REGENCY CAKESTAND 3 TIER` is the single highest revenue-generating item (>£164K net revenue), offering an optimal balance of price point (~£11–12) and demand volume.

---

## 🚀 7. Business Recommendations

Based on the dashboard insights, the following strategic actions are recommended:

1. **Implement a VIP Customer Loyalty & Retention Program:**
   * Create dedicated key account management teams for the top 50 revenue-generating customers (e.g., Accounts `14646`, `18102`, `17450`) to protect high-margin volume.
2. **Optimize Q4 Inventory & Supply Chain Operations:**
   * Increase safety stock levels by at least **45%** starting in August to prepare for the proven Q4 holiday sales surge (September–November).
3. **Target Expansion Marketing in Western Europe:**
   * Focus international ad spend and localized logistics setups in the Netherlands, Germany, and Ireland, where organic product market fit is already proven.
4. **Bundle Low-Volume / High-Price Products:**
   * Use high-volume impulse items (like lighting and heart ornaments) to cross-sell higher-priced inventory identified on the scatter chart.

---

## 📂 8. Repository Structure

```text
.
├── data/
│   └── UK_Online_Retail_Cleaned.csv         # Cleaned transaction dataset
├── docs/
│   ├── Page1_Executive_Overview.png         # High-res dashboard screenshot (Page 1)
│   └── Page2_Product_Customer_Analysis.png  # High-res dashboard screenshot (Page 2)
├── UK_Retail_Analytics.pbix                 # Main Power BI Desktop report file
├── UK_Retail_Executive_Report.pdf           # Executive PDF export of the report
└── README.md                                # Detailed project case study documentation
```

---

## 🛠️ 9. How to Setup & View This Project

1. **Clone the Repository:**
   ```bash
   git clone https://github.com/your-username/UK-Retail-PowerBI-Analytics.git
   ```
2. **Open in Power BI Desktop:**
   * Download and install the latest version of [Microsoft Power BI Desktop](https://powerbi.microsoft.com/).
   * Open `UK_Retail_Analytics.pbix`.
3. **Explore Static Exports:**
   * View `UK_Retail_Executive_Report.pdf` in the root directory for a ready-to-view PDF presentation.
   * View high-resolution preview images inside the `/docs` folder.

---

## 👤 Author & Contact

* **Developer:** Lakhveer Singh
* **Tools Used:** Power BI Desktop, Power Query (M), DAX, Data Modeling, UI/UX Design
