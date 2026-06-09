# 🛍️ Shopify Analysis on Power BI| Sales & Customer Funnel Report

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![Excel](https://img.shields.io/badge/Microsoft%20Excel-217346?style=for-the-badge&logo=microsoft-excel&logoColor=white)
![DAX](https://img.shields.io/badge/DAX-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)

An interactive Power BI dashboard analyzing Shopify e-commerce sales data across **7,431 orders** spanning one week of US operations. The report covers transaction performance, customer behavior, regional trends, payment preferences, and product insights — designed to support data-driven business decisions.

---

## 📊 Dashboard

### Page 1 — Full Analysis View
![Dashboard Main](assets/dashboard_main.png)

### Page 2 — Filtered View (Shopify Payments | Net Sales)
![Dashboard Filtered](assets/dashboard_filtered.png)

### Page 3 — Dynamic Measure View (Total Customers)
![Dashboard Customers](assets/dashboard_customers.png)

---

## 🎯 Business Objective

Analyze Shopify sales data to uncover meaningful insights across three core areas:

| Area | Focus |
|---|---|
| **Transaction Performance** | Revenue, volume, and average order value |
| **Customer Behavior** | Loyalty, single vs. repeat purchase patterns |
| **Retention & Value** | Lifetime value, repeat rate, purchase frequency |

The dashboard enables stakeholders to identify patterns in revenue generation, customer retention, and regional engagement — with full interactivity through dynamic measure switching and drill-through.

---

## 📁 Repository Structure

```
shopify-powerbi/
│
├── README.md
├── assets/                          ← Screenshots used in this README
├── data/                            ← Source dataset (Excel)
├── powerbi/                         ← Power BI report file (.pbix)
├── business_requirements/           ← Original requirements deck (PPT)
├── data_dictionary/                 ← Field definitions and terminology (Word)
└── dax/
    └── measures.md                  ← All DAX measures documented
```

---

## 📋 Dataset Overview

- **Source:** Shopify e-commerce export (anonymized/dummy data)
- **Period:** March 18–24 (one week)
- **Rows:** 7,431 orders
- **Columns:** 19 fields

| Field | Description |
|---|---|
| `Order Number` | Unique order identifier |
| `Invoice Date` | Date and time of order placement |
| `Customer Id` | Unique customer ID (repeats for returning customers) |
| `Billing Address Province` | US state of the customer |
| `CITY` | City of the customer |
| `Gateway` | Payment method used |
| `Product Type` | Category of product purchased |
| `Quantity` | Units ordered |
| `Subtotal Price` | Revenue before tax — used for all Net Sales calculations |
| `Total Price USD` | Revenue after tax |
| `Total Tax` | Tax amount applied |

---

## 📐 KPIs & Measures

### Transaction Performance
| KPI | Value (Full Dataset) |
|---|---|
| Net Sales | $4.2M |
| Total Quantity | 7.5K |
| Average Order Value | $562.6 |

### Customer Purchase Behavior
| KPI | Value |
|---|---|
| Total Customers | 4K |
| Single Order Customers | 2K |
| Repeat Customers | 2K |

### Retention & Value
| KPI | Value |
|---|---|
| Lifetime Value | $943.55 |
| Repeat Rate | 46% |
| Purchase Frequency | 1.68 |

> All DAX measures with full explanations are documented in [`dax/measures.md`](dax/measures.md)

---

## 📊 Dashboard Features

- **Dynamic Measure Switching** — All charts update across Net Sales, Total Quantity, Total Customers, and Repeat Customers via a single slicer
- **Regional Analysis** — Province-level choropleth map + city-level bubble density map with tooltips
- **Sales Trend** — Daily area chart and hourly bar chart to identify peak activity windows
- **Payment Gateway Breakdown** — Donut chart showing customer payment preferences
- **Product Performance** — Column chart identifying best and least performing product types
- **Drill-Through** — Right-click any chart element to navigate to a row-level detail table
- **Dynamic Titles** — Chart titles auto-update based on the selected measure
- **Cross-Chart Filtering** — Selecting any visual filters the entire dashboard

---

## 💡 Key Insights

- **Top states:** Texas, California, Florida, and New York drive the majority of revenue
- **Top cities:** Washington DC, Houston, and El Paso lead in Net Sales
- **Peak hours:** Sales drop from midnight to 7 AM and peak through late morning to evening
- **Payment preference:** Shopify Payments dominates at ~58%, followed by PayPal at ~17%
- **Top products:** Running Shoes → Tennis Shoes → Walking Shoes (classic Pareto distribution)
- **Repeat rate:** 46% of customers are repeat buyers — a strong loyalty signal for a one-week window
- **LTV:** Each customer contributes ~$943 in revenue over the data period

---

## 🛠️ Tools & Techniques

| Tool / Feature | Usage |
|---|---|
| Power BI Desktop | Report development and visualization |
| Power Query | Data quality checks — column validity and error detection |
| DAX | All KPI calculations, calculated columns, dynamic measures |
| Field Parameters | Dynamic measure switching across all visuals |
| Shape Map | US province-level choropleth |
| Bubble Map | City-level density map with custom location column |
| Drill-Through | Row-level detail navigation from any chart |
| Page Navigator | Seamless navigation between report pages |

---

## 🚀 How to Use

1. Download `ShopifySalesReport.pbix` from the `powerbi/` folder
2. Open in **Power BI Desktop** *(free — [download here](https://powerbi.microsoft.com/desktop/))*
3. If prompted, update the data source path to point to `data/shopify_sales.xlsx`
4. Use the **Select Measure** slicer to switch between Net Sales, Quantity, Customers, and Repeat Customers
5. Click any chart element and select **Drill through → Details Tab** to explore row-level data

---

*Dataset is anonymized dummy data based on a real Shopify store. No proprietary business information is disclosed.*


