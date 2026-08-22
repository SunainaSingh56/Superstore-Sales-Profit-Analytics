# 📊 Superstore Sales & Profit Analytics — Power BI Dashboard

<p align="center">
  <img src="https://img.shields.io/badge/Tool-Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black"/>
  <img src="https://img.shields.io/badge/Data-Superstore%20Dataset-0078D4?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Model-Star%20Schema-6A0DAD?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Status-Completed-28A745?style=for-the-badge"/>
</p>

---

## 🧭 Table of Contents

1. [Project Overview](#-project-overview)
2. [Business Problem](#-business-problem)
3. [Dashboard Pages](#-dashboard-pages)
4. [Key Insights](#-key-insights)
5. [Data Model](#-data-model--star-schema)
6. [DAX Measures Used](#-dax-measures-used)
7. [Tech Stack](#-tech-stack)
8. [Dataset](#-dataset)
9. [Project Structure](#-project-structure)
10. [How to Use](#-how-to-use)
11. [Connect with Me](#-connect-with-me)

---

## 🔍 Project Overview

This end-to-end **Power BI Analytics Dashboard** transforms raw transactional retail data from the Superstore dataset into a **multi-page, interactive business intelligence solution**. The project covers the complete analytics workflow — from data cleaning and star schema modelling to DAX-powered KPIs and interactive storytelling.

> **Goal:** Enable business stakeholders to quickly identify sales trends, high-performing segments, and loss-making products — all within a single, filterable dashboard.

---

## 💼 Business Problem

A retail superstore wants to answer the following questions using 4 years of sales data (2014–2017):

- Which **product categories and sub-categories** are driving profit — and which are bleeding it?
- Which **customer segments and regions** contribute most to revenue?
- Are **discounts hurting profitability** across any category?
- Which **products are consistently loss-making** and need review?
- How does **monthly performance** trend across the year?

---

## 📋 Dashboard Pages

### Page 1 — Sales Overview

> High-level performance snapshot with geographic and segment-level breakdown.

| Visual | Description |
|---|---|
| **KPI Cards** | Total Sales (2.30M), Total Profit (286K), Profit Margin (12.47%), Total Orders (5K), Total Customers (793) |
| **Sales by Segment** | Donut chart — Consumer (1161.4K) leads, followed by Corporate (706.15K) and Home Office (429.65K) |
| **Sales by Region** | Donut chart — West (764.63K) > East (611.73K) > Central (518.8K) > South (402.03K) |
| **Total Sales by State** | Bing Maps choropleth across all US states |
| **Sales by Category** | Bar chart — Technology (836K), Furniture (742K), Office Supplies (719K) |
| **Sales by Month** | Line chart showing seasonal trends — peak in November (352K) |
| **Slicers** | Year (2014–2017), Region (Central/East/South/West), Category |

---

### Page 2 — Profit & Loss Analysis

> Deep-dive into profitability, loss-making products, top performers, and shipping efficiency.

| Visual | Description |
|---|---|
| **KPI Cards** | Loss-Making Products (1.36K), Avg Discount (15.62%), Total Profit (286K), Profit Margin (12.47%), Avg Ship Days (3.96) |
| **Top 5 Products by Sales** | Bar chart — Canon imageCLASS leads at 62K |
| **Total Profit by Month** | Line chart tracking profit trends across 12 months |
| **Sales by Ship Mode** | Bar chart — Standard Class dominates (1358K) |
| **Profit by Sub-Category** | Horizontal bars — Copiers (56K), Phones (45K), Accessories (42K) top performers |
| **Profit by Category** | Bar chart — Technology (145K), Office Supplies (122K), Furniture (low/loss) |
| **Loss-Making Products Table** | Detailed table of products with negative profit, filterable by month |
| **Slicer** | Month slicer (Jan–Dec) for granular analysis |

---

## 💡 Key Insights

- 🏆 **Technology** is the top revenue **and** profit category; Furniture has the lowest profit margin despite high sales
- 📍 **West region** generates the highest sales (33%+), while **South** is the weakest performer
- 🛒 **Consumer segment** drives 50%+ of total sales — the primary revenue engine
- 📦 **Standard Class shipping** accounts for 59% of all shipments — signals cost-efficiency opportunity
- 📉 **1,360+ products are loss-making** — largely in Office Supplies and Furniture sub-categories
- 🏷️ **15.62% average discount rate** is directly correlated with low/negative margins in Furniture
- 📈 **November–December** are consistently the highest sales months across all years (holiday surge)
- 🔧 **Copiers and Phones** are the most profitable sub-categories; **Tables and Bookcases** are chronic loss-makers

---

## 🗂️ Data Model — Star Schema

```
                    ┌─────────────────────┐
                    │    dim_Customer      │
                    │  CustomerID (PK)     │
                    │  CustomerName        │
                    │  Segment             │
                    └──────────┬──────────┘
                               │
┌─────────────────┐            │            ┌─────────────────────┐
│   dim_Product   │            ▼            │     dim_Date         │
│  ProductID (PK) ├──→  fact_Orders  ←──── │  DateID (PK)         │
│  ProductName    │            │            │  Year / Month        │
│  Category       │            │            │  Quarter             │
│  Sub-Category   │            │            └─────────────────────┘
└─────────────────┘            │
                               ▼
                    ┌──────────────────────┐
                    │     dim_Location      │
                    │  PostalCode (PK)      │
                    │  City / State         │
                    │  Region               │
                    └──────────────────────┘
```

**fact_Orders** contains: `OrderID`, `OrderDate`, `Sales`, `Profit`, `Discount`, `Quantity`, `ShipMode`, `ShipDate`

---

## 📐 DAX Measures Used

```DAX
-- Core KPIs
Total Sales      = SUM(fact_Orders[Sales])
Total Profit     = SUM(fact_Orders[Profit])
Profit Margin %  = DIVIDE([Total Profit], [Total Sales], 0)
Total Orders     = DISTINCTCOUNT(fact_Orders[OrderID])
Total Customers  = DISTINCTCOUNT(fact_Orders[CustomerID])

-- Loss Analysis
Avg Discount     = AVERAGE(fact_Orders[Discount])
Avg Ship Days    = AVERAGEX(fact_Orders, DATEDIFF(fact_Orders[OrderDate], fact_Orders[ShipDate], DAY))
Loss Products    = CALCULATE(DISTINCTCOUNT(fact_Orders[ProductID]), fact_Orders[Profit] < 0)
```

## 📸 Dashboard Preview

### Page 1 — Sales Overview
![Sales Overview](Screenshots/Page1_Sales_Overview.png)

### Page 2 — Profit & Loss Analysis
![Profit Analysis](Screenshots/Page2_Profit_Analysis.png)

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| **Power BI Desktop** | Data modelling, DAX, dashboard building |
| **Power Query (M)** | Data cleaning, type formatting, column transformations |
| **DAX** | KPI measures, time intelligence, conditional logic |
| **Bing Maps** | Geographic visualisation (Sales by State) |
| **Star Schema** | Relational data model for performance and scalability |

---

## 📁 Dataset

| Attribute | Detail |
|---|---|
| **Source** | [Kaggle — Superstore Sales Dataset](https://www.kaggle.com/datasets/vivek468/superstore-dataset-final) |
| **Records** | ~9,994 rows |
| **Period** | 2014 – 2017 |
| **Fields** | 21 columns — Orders, Customers, Products, Geography, Sales, Profit, Shipping |

---

## 📂 Project Structure

```
superstore-powerbi-dashboard/
│
├── 📊 Superstore_Sales_Analytics.pbix    # Main Power BI file
├── 📁 Dataset/
│   └── Sample_Superstore.csv             # Raw source data
├── 📁 Screenshots/
│   ├── Page1_Sales_Overview.png          # Dashboard page 1
│   └── Page2_Profit_Analysis.png         # Dashboard page 2
└── 📄 README.md
```

---

## ▶️ How to Use

1. **Clone this repository**
   ```bash
   git clone https://github.com/SunainaSingh56/superstore-powerbi-dashboard.git
   ```

2. **Open the `.pbix` file** in Power BI Desktop (free download from Microsoft)

3. **Interact with the dashboard:**
   - Use **Year buttons** (2014–2017) to filter by time period
   - Use **Region slicer** (Page 1) to drill into geographic performance
   - Use **Month slicer** (Page 2) to analyse monthly profit trends
   - Use **Category dropdown** to filter all visuals simultaneously

> 💡 *No data refresh required — dataset is embedded in the `.pbix` file*

---

## 🙋‍♀️ Connect with Me

**Sunaina Singh**
*Aspiring Data Analyst | Power BI · SQL · Python · Excel*

[![LinkedIn](https://img.shields.io/badge/LinkedIn-sunainasingh56-0A66C2?style=for-the-badge&logo=linkedin)](https://linkedin.com/in/sunainasingh56)
[![GitHub](https://img.shields.io/badge/GitHub-SunainaSingh56-181717?style=for-the-badge&logo=github)](https://github.com/SunainaSingh56)

---

<p align="center">
  <i>⭐ If you found this project helpful, do give it a star — it means a lot!</i>
</p>
