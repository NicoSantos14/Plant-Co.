# Plant-Co.
This Power BI dashboard for fictional Plant Co. transforms 2022–2024 sales data into a single, powerful interactive page. Switch between total sales, gross profit, or units sold and see instant year-over-year comparisons. A simple dropdown refreshes every chart, and clear visuals uncover key trends to help managers act faster.

![Dashboard Overview](https://github.com/NicoSantos14/Plant-Co./blob/0b91055203d2afb5fc31739b3f8b83b4da4a677b/Screenshot%202026-02-05%20174612.png)


# Power BI Sales Performance Dashboard – Portfolio Project

![Power BI Dashboard Screenshot](screenshots/dashboard-overview.png)  
*(Replace with your actual screenshot path – add images to a /screenshots folder)*

An end-to-end Power BI project demonstrating data cleaning, modeling, advanced DAX, dynamic interactivity, and insightful visualizations. Built using a fictional plant sales dataset to analyze sales, gross profit, quantity trends, and year-over-year performance.

This project is ideal for data analyst portfolios: it showcases real-world skills employers value, including Power Query transformations, star-schema modeling, time intelligence, parameter-based switching, conditional formatting, and business-oriented insights.

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

Download the data file from this repo or use your own similar dataset.

## Step-by-Step Build Guide

### 1. Data Import & Cleaning (Power Query)

1. Open Power BI Desktop → Get Data → Excel → Select `Plant_DTS.xls`
2. Load tables: Accounts, Plant Fact, Plant Hierarchy
3. In Power Query Editor:
   - Rename: Plant Hierarchy → **Dim Product**, Accounts → **Dim Account**, Plant Fact → **Fact Sales**
   - Dim Product: Remove duplicates on Product Name (unique key)
   - Dim Account: Remove duplicates on Account ID; rename Latitude 2 → Latitude, Country 2 → Country
   - Fact Sales: Change Date to Date type (not Date/Time); verify numeric types for Sales, Quantity, COGS
4. Close & Apply

### 2. Supporting Tables

- **Date Table** (Modeling → New Table):
  ```dax
  Dim Date = CALENDAR(DATE(2022, 1, 1), DATE(2024, 12, 31))
