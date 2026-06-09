# DAX Measures — Shopify Sales & Customer Funnel Report

All measures are written in DAX (Data Analysis Expressions) and built on a single table: `shopify_sales`.

---

## 📦 Table of Contents

1. [Transaction Performance](#1-transaction-performance)
2. [Customer Purchase Behavior](#2-customer-purchase-behavior)
3. [Retention & Value KPIs](#3-retention--value-kpis)
4. [Calculated Columns](#4-calculated-columns)
5. [Dynamic Titles](#5-dynamic-titles)

---

## 1. Transaction Performance

These measures capture the overall health of sales operations.

---

### Net Sales
> Total revenue generated **before tax**. Uses `Subtotal Price` field, not `Total Price USD`, to exclude tax from revenue figures.

```dax
Net Sales = SUM(shopify_sales[Subtotal Price])
```

---

### Total Quantity
> Total number of product units sold across all orders in the selected period.

```dax
Total Quantity = SUM(shopify_sales[Quantity])
```

---

### Average Order Value
> Average revenue per transaction before tax. Each row in the dataset represents one line item, so averaging `Subtotal Price` gives revenue per order.

```dax
Average Order Value = AVERAGE(shopify_sales[Subtotal Price])
```

---

## 2. Customer Purchase Behavior

These measures analyze how customers interact with the business — critical for any e-commerce or B2C operation.

---

### Total Customers
> Count of **unique** customers. `Customer Id` can repeat in the dataset (returning customers place multiple orders), so `DISTINCTCOUNT` is used to avoid double-counting.

```dax
Total Customers = DISTINCTCOUNT(shopify_sales[Customer Id])
```

---

### Single Order Customers
> Customers who placed **exactly one order**. Uses a filter on unique Customer IDs where the distinct count of their Order Numbers equals 1.

```dax
Single Order Customers =
COUNTROWS(
    FILTER(
        VALUES(shopify_sales[Customer Id]),
        CALCULATE(DISTINCTCOUNT(shopify_sales[Order Number])) = 1
    )
)
```

**Logic:** `VALUES()` returns each unique Customer ID. `FILTER()` keeps only those where their order count equals 1. `COUNTROWS()` then counts how many customers meet that condition.

---

### Repeat Customers
> Customers who placed **more than one order** — an indicator of loyalty and product satisfaction.

```dax
Repeat Customers =
COUNTROWS(
    FILTER(
        VALUES(shopify_sales[Customer Id]),
        CALCULATE(DISTINCTCOUNT(shopify_sales[Order Number])) > 1
    )
)
```

---

## 3. Retention & Value KPIs

These measures support long-term business growth analysis.

---

### Lifetime Value (LTV)
> Average revenue contributed per customer over the data period. Divides total net revenue by the number of unique customers.

```dax
Life Time Value = [Net Sales] / [Total Customers]
```

---

### Repeat Rate
> Percentage of customers who returned to make another purchase. A higher repeat rate signals stronger brand loyalty.

```dax
Repeat Rate = [Repeat Customers] / [Total Customers]
```

> Formatted as percentage in the report. Result: **~46%** repeat rate in the sample dataset.

---

### Purchase Frequency
> Average number of orders placed per customer. Uses `DISTINCTCOUNT` of Order Number to count unique orders before dividing by customer count.

```dax
Purchase Frequency = 
DISTINCTCOUNT(shopify_sales[Order Number]) / [Total Customers]
```

> Result: **~1.67** — meaning each customer places an average of 1.67 orders in the week.

---

## 4. Calculated Columns

These columns are added directly to the `shopify_sales` table in Power BI.

---

### Hour
> Extracts the hour (0–23) from the `Invoice Date` timestamp. Used to plot the hourly sales activity bar chart.

```dax
Hour = HOUR(shopify_sales[Invoice Date])
```

---

### Location
> Combines City, Province, and Country into one string to help Power BI accurately plot city-level bubbles on the map. Without this, Power BI misidentifies cities across different countries.

```dax
Location =
shopify_sales[CITY] & ", " &
shopify_sales[Billing Address Province] & ", " &
shopify_sales[Billing Address Country]
```

**Example output:** `HOUSTON, Texas, United States`

---

### Customer Name
> Combines first and last name into a single readable field for the Details tab grid.

```dax
Customer Name =
shopify_sales[Billing Address First Name] & " " &
shopify_sales[Billing Address Last Name]
```

---

## 5. Dynamic Titles

These measures power the chart titles that update automatically when the user switches the measure slicer.

---

### Locations Title
> Drives the dynamic title on the regional overview section. Uses `SWITCH` on the parameter order index to return the correct label.

```dax
Locations title =
SWITCH(
    SELECTEDVALUE('Select Measure'[Select Measure Order]),
    0, "Net Sales by location",
    1, "Total Quantity by location",
    2, "Total Customers by location",
    3, "Repeat Rate by location"
)
```

---

### Trend Title
> Drives the dynamic title on the Sales Trend over Time charts.

```dax
Trend Title =
SELECTEDVALUE('Select Measure'[Dynamic Type]) & " Trend over Time"
```

---

### Gateway Title
> Drives the dynamic title on the payment gateway donut chart.

```dax
Gateway Title =
SELECTEDVALUE('Select Measure'[Dynamic Type]) & " by Gateway Payment Method"
```

---

### Product Title
> Drives the dynamic title on the product type column chart.

```dax
Product Title =
SELECTEDVALUE('Select Measure'[Dynamic Type]) & " by Product Type"
```

---

## 📌 Notes

- All revenue measures use `Subtotal Price` (pre-tax) not `Total Price USD` to ensure net figures
- `Customer Id` is not unique per row — `DISTINCTCOUNT` is always used for customer counts
- The `Select Measure` field parameter was created via **Modeling → New Parameter → Fields** in Power BI Desktop, enabling dynamic measure switching across all charts

