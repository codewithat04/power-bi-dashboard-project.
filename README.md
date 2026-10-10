RetailPulse Sales Analytics (Power BI)
Author: Ayushmaan
Course: Business Analytics / Descriptive Analytics
Tools: Power BI Desktop (Power Query, Model view, DAX), Power BI Service
Project Overview
Practical
Topic
What was done
7
Import & Transform
Loaded Excel data, set data types, removed blanks/duplicates, replaced nulls, added a conditional column Sale Size, Close & Apply
8
Model, DAX & Publish
Built one-to-many relationships, created measures and a calculated column, designed an interactive dashboard, published to Power BI Service
9
Time Intelligence & Performance
Created a Date table, YTD / last-year / YoY measures, Region Share % with ALL, matrix visual, Performance Analyzer
Dataset
Source file: RetailPulse_Sales.xlsx with three tables.
Table
Rows
Description
Sales
1,800
Fact table: OrderID, Sales Date, ProductID, StoreID, Quantity, Sales Amount
Product
16
Dimension: Product Name, Product Category, Unit Price
Store
8
Dimension: Store Name, Region
Date range: 01-Jan-2024 to 31-Dec-2025
Product categories: Electronics, Clothing, Home & Kitchen, Grocery
Regions: North, South, East, West
Data quality: 0 nulls, 0 duplicates, 0 error values
Data Model
Star schema with single-direction, one-to-many relationships:
Product (1) ──< Sales (fact) >── (1) Store
                  │
              Date (1) ──< Sales[Sales Date]
Product[ProductID] → Sales[ProductID]
Store[StoreID] → Sales[StoreID]
Date[Date] → Sales[Sales Date] (Date table marked as date table)
Power Query Transformations (Practical 7)
Changed types: Sales Date → Date, Quantity → Whole Number, Sales Amount → Fixed decimal number
Removed blank rows and duplicates
Replaced null Quantity values with 0
Added conditional column Sale Size: Large if Sales Amount ≥ 1000, otherwise Standard
Large: 127 rows, Standard: 1,673 rows
DAX Used
-- Practical 8
Total Sales         = SUM(Sales[Sales Amount])
Total Quantity      = SUM(Sales[Quantity])
Average Order Value = DIVIDE([Total Sales], DISTINCTCOUNT(Sales[OrderID]))
Sales per Unit      = DIVIDE(Sales[Sales Amount], Sales[Quantity])   -- calculated column

-- Practical 9
Date = CALENDAR(MIN(Sales[Sales Date]), MAX(Sales[Sales Date]))

Sales YTD = TOTALYTD([Total Sales], 'Date'[Date])

Sales LY  = CALCULATE([Total Sales], SAMEPERIODLASTYEAR('Date'[Date]))

YoY Growth % =
VAR Curr = [Total Sales]
VAR Prev = [Sales LY]
RETURN DIVIDE(Curr - Prev, Prev)

Region Share % =
DIVIDE([Total Sales], CALCULATE([Total Sales], ALL(Store[Region])))
Key Results
Overall KPIs
Metric
Value
Total Sales
$592,436.13
Total Quantity
5,399
Average Order Value
$329.13
Average Sales per Unit
$114.85
Sales by Region
Region
Total Sales
Share
North
$160,508.22
27.1%
South
$154,544.14
26.1%
East
$149,353.45
25.2%
West
$128,030.32
21.6%
Year over Year
Year
Total Sales
2024
$270,087
2025
$322,349 (+19.4% YoY)
Insights
Electronics is the largest category by a wide margin (about $368K of $592K).
North is the top region; West is the lowest.
December 2024 was the peak month; September 2024 was the lowest.
2025 grew 19.4% over 2024 overall.
Dashboard
The report page includes:
KPI cards: Total Sales, Total Quantity, Average Order Value
Bar chart: Total Sales by Product Category
Line chart: Total Sales by Month
Region slicer with cross-filtering across all visuals
Matrix: Year / Month with Total Sales, Sales YTD, Sales LY, YoY Growth %
Region Share % chart
Interactive version: https://claude.ai/artifact/7pSirGD5aeXowDYwjKMSTG
Repository Structure
.
├── README.md
├── data/
│   └── RetailPulse_Sales.xlsx
├── powerbi/
│   └── RetailPulse_Analysis.pbix
├── report/
│   └── Power_BI_Practicals_7-9.pdf
└── images/
    └── dashboard.png
How to Open
Install Power BI Desktop.
Clone this repository.
Open powerbi/RetailPulse_Analysis.pbix.
If prompted, point the data source to data/RetailPulse_Sales.xlsx (Home → Transform data → Data source settings).
Concepts Covered
Import vs DirectQuery, Applied Steps, data types, star schema, measures vs calculated columns, DIVIDE, CALCULATE, ALL, VAR
