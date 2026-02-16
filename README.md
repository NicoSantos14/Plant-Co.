# Plant-Co.
This Power BI dashboard for fictional Plant Co. transforms 2022–2024 sales data into a single, powerful interactive page. Switch between total sales, gross profit, or units sold and see instant year-over-year comparisons. A simple dropdown refreshes every chart, and clear visuals uncover key trends to help managers act faster.


# Power BI Sales Performance Dashboard – Portfolio Project
![Dashboard Overview](https://github.com/NicoSantos14/Plant-Co./blob/0b91055203d2afb5fc31739b3f8b83b4da4a677b/Screenshot%202026-02-05%20174612.png)


An end-to-end Power BI project demonstrating data cleaning, modeling, advanced DAX, dynamic interactivity, and insightful visualizations. Built using a fictional plant sales dataset to analyze sales, gross profit, quantity trends, and year-over-year performance.


## Project Overview

**Business Objective**  
Create an interactive performance dashboard that allows users to dynamically switch between key metrics (Sales, Gross Profit, Quantity) and compare current vs. prior year-to-date performance. Highlight growth/decline areas, product hierarchies, geographic trends, and variances.

**Key Features**
- Dynamic metric switching via slicer (no duplicate pages needed)
- Accurate YTD comparisons with future-month filtering
- Conditional formatting for instant insight detection
- Advanced visuals: treemaps, waterfalls, combo charts, geographic scatter
- Clean data model and organized measures

**Tools & Technologies**
- Power BI Desktop
- Power Query (data transformation)
- DAX (measures & calculated columns)
- Excel (source data)

## Dataset

- **Source**: `Plant_DTS.xls` (Excel workbook)
- **Tables**:
  - Fact Sales – Transaction-level sales data (Date, Product ID, Account ID, Sales, Quantity, COGS, etc.)
  - Dim Account – Customer/account details (Account ID, Country, Latitude, Longitude, etc.)
  - Dim Product – Product hierarchy (Product ID, Product Name, Category, etc.)
- **Time Range**: 2022–2024 (partial)

## Set Up Model & Measures
# Power BI / DAX - Dynamic Year-to-Date Metrics with Slicer Control

This folder contains a clean, reusable DAX pattern for showing **current** and **prior year-to-date (YTD)** values with a dynamic metric selector (Sales, Gross Profit, Quantity).

## Data Model Assumptions

- Fact table: `'Fact Sales'` with columns: `Sales`, `Quantity`, `COGS`
- Date table: `'Dim Date'` marked as date table, with column `Date`
- `'Dim Date'[In Past]` = `TRUE` for all dates up to and including today (helps avoid future dates in YTD)
- Slicer table: `'Slicer Values'` (disconnected) with one column `Values` containing at least:
  - "Sales"
  - "Gross Profit"
  - "Quantity"

## Base Measures

Sales =
SUM('Fact Sales'[Sales])

Quantity =
SUM('Fact Sales'[Quantity])

COGS =
SUM('Fact Sales'[COGS])

Gross Profit =
[Sales] - [COGS]



## Prior YTD

Prior YTD Sales =
CALCULATE(
    [Sales],
    SAMEPERIODLASTYEAR('Dim Date'[Date]),
    'Dim Date'[In Past] = TRUE()
)


# Power BI Portfolio Project: Plant Sales Performance Dashboard

**Nick (Nicolas Nunez)**  
Data Analyst | Power BI • DAX • Power Query  
Palisades Park, NJ / New York City

---

## Project Overview

Interactive Power BI dashboard analyzing sales, gross profit, and quantity trends (2022–2024) for a fictional plant/nursery company.

Key features demonstrated:
- Single slicer to dynamically switch between **Sales**, **Gross Profit**, and **Quantity**
- Current vs Prior Year-to-Date (YTD) comparison
- YoY variance analysis with waterfall charts
- Product-level contribution (treemaps)
- Geographic performance insights (bubble scatter map)
- Conditional formatting, tooltips, and clean layout

Great showcase for:
- Data modeling & relationships
- Power Query cleaning
- Advanced DAX (time intelligence + dynamic measures)
- Professional dashboard design

### Dashboard Preview
![Plant Sales Performance Dashboard Overview](https://github.com/NicoSantos14/Plant-Co./raw/dbb072702875871b8003f3b3b522c4b086f504bb/Screenshot%202026-02-05%20174612.png)

*(Full dashboard view showing dynamic metric switching, key KPIs, treemap, waterfall variance, trend combo chart, and geographic scatter map)*

---

## Repository Contents
plant-sales-powerbi/
├── Performance Report.pbix     → Main Power BI file
├── Plant_DTS.xls               → Raw source Excel data
├── screenshots/                → Additional dashboard captures (optional)
└── README.md                   → This file
text---

## Quick Setup & Recreation Steps

### 1. Load and Clean Data (Power Query)

1. Open **Power BI Desktop**
2. **Get Data → Excel** → select `Plant_DTS.xls`
3. In Power Query Editor:
   - Rename queries: `Dim Product`, `Dim Account`, `Fact Sales`
   - Remove duplicates:
     - `Dim Product`: Product Name
     - `Dim Account`: Account ID
   - Change column types:
     - Date → Date
     - Sales, Quantity, COGS → Decimal Number
   - **Close & Apply**

### 2. Create Helper Tables

- **Date table** (create via DAX):
  ```dax
  Dim Date = 
  CALENDAR(DATE(2022, 1, 1), DATE(2024, 12, 31))

Past periods flag (for accurate prior YTD):daxIn Past = 
VAR LastVisibleDate = MAX('Fact Sales'[Date])
RETURN 'Dim Date'[Date] <= EOMONTH(LastVisibleDate, -12)
Metric slicer table (manual – use "Enter Data"):
Column name: Values
Rows:
Sales
Gross Profit
Quantity


3. Set Up Model & Relationships

Fact Sales[Product ID] → Dim Product[Product ID] (1:*)
Fact Sales[Account ID] → Dim Account[Account ID] (1:*)
Fact Sales[Date] → Dim Date[Date] (1:*)

Mark Dim Date as the date table.
4. Core DAX Measures
Base measures
daxSales        = SUM('Fact Sales'[Sales])
Quantity     = SUM('Fact Sales'[Quantity])
COGS         = SUM('Fact Sales'[COGS])
Gross Profit = [Sales] - [COGS]
Prior YTD examples (repeat pattern for each metric)
daxPrior YTD Sales = 
CALCULATE(
    [Sales],
    SAMEPERIODLASTYEAR('Dim Date'[Date]),
    'Dim Date'[In Past] = TRUE
)
Dynamic switching measures
daxSelected Metric = 
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
5. Build the Dashboard

Add single-select slicer using Slicer Values[Values]
Use [Selected Metric] and [Prior YTD Selected] in visuals
Recommended visuals:
KPI Cards (current value + % change)
Treemap (by product)
Waterfall chart (variance explanation)
Line + Column combo chart (trend current vs prior)
Bubble scatter map (by country/region)

Add conditional formatting (green = positive variance, red = negative)
Include product/category and country slicers
Enhance with bookmarks, tooltips, and subtle theme


Key Insights (Typical Patterns Seen)

Q2 gross profit often drops ~12–18% YoY due to rising COGS
Top 5–7 products typically drive 55–70% of revenue
Strong growth in specific regions/countries; others flat or declining
Seasonal patterns visible in outdoor vs indoor plant categories


Tools & Skills Showcased

Power BI Desktop
Power Query (ETL, cleaning, type consistency)
DAX:
Time intelligence (SAMEPERIODLASTYEAR, EOMONTH)
Dynamic measures with SWITCH
Context manipulation

Visual design: treemaps, waterfalls, combo charts, scatter maps
Conditional formatting, slicers, tooltips


How to Use This Repository

Clone or download the repo
Open Performance Report.pbix in Power BI Desktop
Explore the slicers and interactions
Feel free to customize colors, add drill-through pages, or experiment

MIT License — perfect for portfolios, job applications, learning, or inspiration.
Open to feedback, suggestions, or collaboration ideas!
Nick
Aspiring Data Analyst
Power BI • SQL • Excel
