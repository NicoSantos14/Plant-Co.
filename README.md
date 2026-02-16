<p align="center">
  <h1>Plant Sales Performance Dashboard</h1>
  <h3>Power BI Portfolio Project</h3>
</p>

---

<p align="center">
  <img src="https://github.com/NicoSantos14/Plant-Co./blob/6f4e6869638252443628110e71dc053a444affa7/Screenshot%202026-02-05%20174612.png" width="900">
</p>

---

## Executive Summary

This project presents an end-to-end Power BI dashboard analyzing Sales, Gross Profit, and Quantity for a fictional plant company (2022–2024).

The report demonstrates practical analytics capabilities including data preparation, dimensional modeling, time intelligence, dynamic KPI switching, and executive-level performance reporting.

The goal of the dashboard is to enable rapid identification of trends, margin changes, and underperforming segments to support informed business decisions.

---

## Business Objectives

The dashboard enables stakeholders to:

- Monitor sales and profitability trends  
- Compare current performance versus prior year-to-date  
- Identify gross profit decline caused by rising COGS  
- Evaluate product-level contribution  
- Analyze geographic performance  
- Detect underperforming regions quickly  

---

## Skills Demonstrated

- Data transformation using Power Query  
- Star schema modeling  
- Time intelligence with SAMEPERIODLASTYEAR  
- Controlled date filtering using calculated flags  
- Measure branching and reusable DAX architecture  
- Dynamic metric switching via slicer  
- Conditional formatting for executive visibility  
- Interactive dashboard design  

---

## Dataset Overview

**Source:** `Plant_DTS.xls`  
**Coverage:** January 2022 – April 2024  

### Tables

**Fact Sales**
- Date  
- Product ID  
- Account ID  
- Sales  
- Quantity  
- COGS  
- Price  

**Dim Account**
- Account ID  
- Country  
- Latitude  
- Longitude  

**Dim Product**
- Product ID  
- Product Name  
- Category  

---

## Repository Structure

```
├── Performance Report.pbix
├── Plant_DTS.xls
├── README.md
└── screenshots/
```

---

# Build Process

## 1. Data Preparation (Power Query)

- Imported Excel dataset  
- Renamed tables to dimensional modeling standards  
- Removed duplicate keys  
- Standardized numeric and date data types  
- Cleaned geographic fields  

Result: Structured, analysis-ready dataset.

---

## 2. Data Modeling

A star schema was implemented:

- Fact Sales → Dim Product  
- Fact Sales → Dim Account  
- Fact Sales → Dim Date  

This structure ensures scalable calculations and reliable filter behavior.

---

## 3. Supporting Tables

### Date Table

```DAX
Dim Date =
CALENDAR(DATE(2022, 1, 1), DATE(2024, 12, 31))
```

### In Past Flag (Calculated Column in Dim Date)

```DAX
In Past =
VAR LastSalesDate =
    MAX('Fact Sales'[Date])
VAR LastSalesDatePriorYear =
    EOMONTH(LastSalesDate, -12)
RETURN
    'Dim Date'[Date] <= LastSalesDatePriorYear
```

### Slicer Table

Created using Enter Data:

Values  
- Sales  
- Gross Profit  
- Quantity  

---

## 4. DAX Measures

### Base Measures

```DAX
Sales =
SUM('Fact Sales'[Sales])

Quantity =
SUM('Fact Sales'[Quantity])

COGS =
SUM('Fact Sales'[COGS])

Gross Profit =
[Sales] - [COGS]
```

---

### Prior Year-to-Date Measures

```DAX
Prior YTD Sales =
CALCULATE(
    [Sales],
    SAMEPERIODLASTYEAR('Dim Date'[Date]),
    'Dim Date'[In Past] = TRUE()
)

Prior YTD Gross Profit =
CALCULATE(
    [Gross Profit],
    SAMEPERIODLASTYEAR('Dim Date'[Date]),
    'Dim Date'[In Past] = TRUE()
)

Prior YTD Quantity =
CALCULATE(
    [Quantity],
    SAMEPERIODLASTYEAR('Dim Date'[Date]),
    'Dim Date'[In Past] = TRUE()
)
```

---

### Dynamic Metric Switching

```DAX
Selected Metric =
SWITCH(
    SELECTEDVALUE('Slicer Values'[Values]),
    "Sales",        [Sales],
    "Gross Profit", [Gross Profit],
    "Quantity",     [Quantity],
    BLANK()
)

Prior YTD Selected =
SWITCH(
    SELECTEDVALUE('Slicer Values'[Values]),
    "Sales",        [Prior YTD Sales],
    "Gross Profit", [Prior YTD Gross Profit],
    "Quantity",     [Prior YTD Quantity],
    BLANK()
)
```

---

### Variance Analysis

```DAX
Variance =
[Selected Metric] - [Prior YTD Selected]

Variance % =
DIVIDE(
    [Variance],
    [Prior YTD Selected]
)
```

---

## Dashboard Components

- KPI cards (Current vs Prior YTD)  
- Treemap (Product hierarchy contribution)  
- Waterfall chart (Variance analysis)  
- Combo chart (Trend comparison)  
- Geographic scatter map (Latitude / Longitude)  
- Country slicer  
- Conditional formatting for growth vs decline  
- Cross-filtering interactions  

---

## Example Insights

- Approximately 15% YoY gross profit decline in Q2 driven by rising COGS  
- Top 20% of products contribute approximately 60% of total revenue  
- Geographic analysis reveals expansion opportunities in select markets  

---

## Tools & Technologies

- Power BI Desktop  
- Power Query  
- DAX  
- Dimensional modeling principles  

---

## How to Use This Repository

1. Clone or download the repository  
2. Open `Performance Report.pbix` in Power BI Desktop  
3. Review the data model and DAX measures  
4. Interact with slicers and filters  

---

## Contact

Nicolas Nunez
Leonia, NJ  

If this project was helpful, feel free to star the repository.
