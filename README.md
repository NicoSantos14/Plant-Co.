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
