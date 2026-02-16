Plant Sales Performance Dashboard
Power BI Portfolio Project
<u>Executive Summary</u>

An end-to-end Power BI dashboard analyzing Sales, Gross Profit, and Quantity for a fictional plant company (2022–2024).

This project demonstrates applied analytics skills including data transformation, dimensional modeling, advanced DAX, and dynamic report interactivity. The dashboard is designed to support clear, data-driven business decisions.

<u>Business Objective</u>

This interactive report enables stakeholders to:

Track performance trends over time

Compare Year-to-Date performance versus prior year

Identify underperforming products and regions

Analyze gross profit fluctuations caused by rising COGS

Evaluate geographic performance

The focus is on actionable insight, not just visualization.

<u>Core Skills Demonstrated</u>

Clean data transformation using Power Query

Star schema data modeling

Time intelligence with controlled Prior YTD calculations

Dynamic metric switching (Sales / Gross Profit / Quantity)

Conditional formatting for performance visibility

Executive-level KPI design

Interactive and user-focused dashboard layout

<u>Dataset Overview</u>

Source: Plant_DTS.xls
Coverage: January 2022 – April 2024

Tables

Fact Sales

Date

Product ID

Account ID

Sales

Quantity

COGS

Price

Dim Account

Account ID

Country

Latitude

Longitude

Dim Product

Product ID

Product Name

Category

<u>Repository Structure</u>

├── Performance Report.pbix     # Completed Power BI report
├── Plant_DTS.xls               # Raw dataset
├── README.md                   # Project documentation
└── screenshots/                # Dashboard images
Build Process
<u>1. Data Preparation (Power Query)</u>

Imported Excel dataset

Renamed tables using dimensional modeling standards

Removed duplicate keys

Standardized data types

Cleaned geographic attributes

Result: Structured, analysis-ready dataset.

<u>2. Data Modeling</u>

Implemented a star schema:

Fact Sales → Dim Product

Fact Sales → Dim Account

Fact Sales → Dim Date

This structure ensures scalable DAX calculations and consistent filtering behavior.

<u>3. Key DAX Measures</u>
Base Measures
'''dax
Sales =
SUM('Fact Sales'[Sales])

Quantity =
SUM('Fact Sales'[Quantity])

COGS =
SUM('Fact Sales'[COGS])

Gross Profit =
[Sales] - [COGS]
'''


## Prior Year-to-Date Logic
'''dax
Prior YTD Sales =
CALCULATE(
    [Sales],
    SAMEPERIODLASTYEAR('Dim Date'[Date]),
    'Dim Date'[In Past] = TRUE()
)'''

-This implementation:

-Controls future-date distortion

-Maintains consistent time-intelligence logic

-Supports scalable Year-over-Year comparison

##Dynamic Metric Switching
'''dax
Selected Metric =
SWITCH(
    SELECTEDVALUE('Slicer Values'[Values]),
    "Sales",        [Sales],
    "Gross Profit", [Gross Profit],
    "Quantity",     [Quantity],
    BLANK()
)'''

This enables KPI switching without duplicating report visuals.

<u>Dashboard Components</u>

KPI cards (Current vs Prior YTD)

Treemap (Product hierarchy performance)

Waterfall chart (Variance analysis)

Combo chart (Current vs Prior YTD trends)

Geographic scatter map (Latitude / Longitude)

Conditional formatting for performance signals

Dynamic report titles

Cross-filtering interactions

<u>Example Insights</u>

Approximately 15% YoY gross profit decline in Q2 driven by increased COGS

Top 20% of products generate approximately 60% of total revenue

Geographic analysis reveals expansion opportunities in select regions

<u>Tools & Technologies</u>

Power BI Desktop

Power Query

DAX (Time intelligence, context transition, measure branching)

Dimensional modeling (Star schema design)

<u>How to Use This Repository</u>

Clone or download the repository

Open Performance Report.pbix in Power BI Desktop

Explore the report, model view, and DAX measures

<u>Contact</u>

Nicolas Nunes
Leonia, NJ
