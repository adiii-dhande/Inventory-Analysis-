# Inventory-Analysis-

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)

Inventory Analysis Dashboard | ABC Classification · Pareto Curve · Power BI · DAX
# 📦 Inventory Analysis Dashboard — Power BI

> A single-page, interactive Power BI dashboard for ABC-class inventory analysis, tracking product performance, revenue, profit margin, and inventory turnover across categories.

---

## 🎯 Project Objective

This dashboard was built to answer key inventory management questions:

- Which products generate the most revenue? (ABC Classification)
- How efficiently is stock being turned over per product?
- Which product categories are driving quantity sold and profit margin?
- How does 2021 performance compare to 2020 across products?

---

## 📌 KPI Cards (Top Row)

| KPI | Measure Name | Description |
|---|---|---|
| **Active Products** | `# NumOfProducts` | Total count of active SKUs |
| **Avg Inventory Turnover** | `Avg Inventory turnover` | Average turnover ratio across all products |
| **Quantity Sold** | `Quantity 2021` | Total units sold in 2021 |
| **Revenue** | `$ Revenue 2021` | Total revenue generated in 2021 |
| **Profit Margin** | `Profit Margin 2021` | Overall profit margin % for 2021 |

---

## 📈 Visuals Breakdown

### 1. ABC Class — Area Chart (Pareto / ABC Curve)
- **X-Axis:** Product Rank (`%_cp_revenue_2021_Rank`)
- **Y-Axis:** Cumulative % Revenue (`%_cp_revenue_2021`)
- **Series:** ABC Class (`A`, `B`, `C`)
- **Tooltip:** Product name + individual % revenue
- **Interaction:** Filters the Product table below

**ABC Classification Logic:**
| Class | Cumulative Revenue |
|---|---|
| **A** | < 70% of total revenue |
| **B** | 70% – 90% |
| **C** | > 90% |

---

### 2. ABC Class — Small Multiple Card
- Breaks down `# NumOfProducts` by **ABC Class**
- Allows quick comparison of how many products fall into each tier

---

### 3. Category — Bar Chart
- **Y-Axis:** `Category` (from Stock table)
- **X-Axis:** Dynamic measure — switches between:
  - Quantity Sold
  - Profit Margin
  - Avg Inventory Turnover
- **Slicer buttons** (pill-shaped) control which metric is displayed
- Interacts with the Product table via `DataFilter`

---

### 4. Product — Data Table
- Detailed product-level breakdown with the following columns:

| Column | Source |
|---|---|
| Product Name | `Stock.Description` |
| ABC Class | `Stock.ABC_class` |
| Inventory Turnover | `Avg Inventory turnover` |
| Quantity 2020 | `Quantity 2020` |
| Quantity 2021 | `Quantity 2021` |
| Revenue | `$ Revenue 2021` |
| Profit Margin | `Profit Margin 2021` |
| COGS | `Avg Cogs` |
| Retail Price | `Retail Price` |

---

### 5. Date Range Text Box
- Displays the currently selected date range dynamically using the `Date Text` DAX measure.
- Label: `"Selected Date Range: [value]"`

---

## 🗂️ Data Model

**Tables used:**
- `Stock` — Main product/inventory table containing columns: `Description`, `ABC_class`, `Category`, `%_cp_revenue_2021`, `%_cp_revenue_2021_Rank`, `%_revenue_2021`
- `#Measures` — Dedicated DAX measures table
- `Placeholder` — Used for the dynamic metric slicer on the bar chart (`Placeholder_Text`)

**Key DAX Measures:**

```dax
-- Number of active products
# NumOfProducts = COUNTROWS(Stock)  -- or DISTINCTCOUNT

-- Inventory Turnover
Avg Inventory turnover = AVERAGE(Stock[Inventory Turnover])

-- Revenue (2021)
$ Revenue 2021 = CALCULATE([Revenue], YEAR = 2021)

-- Profit Margin (2021)
Profit Margin 2021 = DIVIDE([Revenue 2021] - [COGS 2021], [Revenue 2021])

-- Dynamic metric switcher for bar chart
Placeholder/Quantity-Margin-AvgTurnOver =
    SWITCH(
        SELECTEDVALUE(Placeholder[Placeholder_Text]),
        "Quantity", [Quantity 2021],
        "Margin",   [Profit Margin 2021],
        "Turnover", [Avg Inventory turnover]
    )

-- Date display text
Date Text = -- Returns selected date range as formatted text
```

---

## 🧩 Report Interactions

| Source Visual | Target Visual | Interaction Type |
|---|---|---|
| ABC Class Area Chart | Product Table | DataFilter |
| Category Bar Chart | Product Table | DataFilter |
| Metric Slicer (pill buttons) | Category Bar Chart | Bookmark/Selection |

---

## 🛠️ Tools & Technologies

| Tool | Usage |
|---|---|
| **Power BI Desktop** | Report building, data modeling, DAX |
| **DAX (Data Analysis Expressions)** | Custom measures and KPIs |
| **Power Query (M Language)** | Data transformation |
| **Custom Background Image** | UI/UX design (PNG embedded in report) |

## Author

**Aditya Dhande**

- Email: adityadhande35@gmail.com
- LinkedIn: https://linkedin.com/in/adiii-dhande
- GitHub: https://github.com/adiii-dhande


