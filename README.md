# Retail-Sales-Performance-Profitability-Analytics

# Retail Sales Performance & Profitability Analytics

An end-to-end Power BI dashboard analyzing retail sales performance and profitability for a retail company, covering 2024-2025 data.

## Business Problem
The company needed a way to track sales performance, profitability, and target achievement across products, customers, and regions — to identify top/bottom performers and regions falling behind targets.

## Dataset
Provided dataset (Retail_Sales_Analytics_Project_Dataset.xlsx) with:
- Raw_Sales — 2,212 rows (including intentional duplicates, nulls, mixed date formats)
- Customers — 500 rows
- Products — 63 rows
- Monthly_Targets — 2024-2025 regional sales targets

## Tools Used
- Power BI Desktop
- Power Query (data cleaning)
- DAX (measures)

## Data Cleaning (Power Query)
15 transformation steps including: deduplication on Order_ID, text trimming/standardization, null handling, date/numeric type conversion, Product_Name/Variant split, derived Order Year/Month columns, and a conditional Order_Status column (Fast/Normal/Delayed based on delivery days).

## Data Model
Star schema:
- FactSales (center)
- DimCustomer, DimProduct, DimDate — 1-to-many relationships to FactSales
- DimMonth — separate from DimDate, used specifically to join MonthlyTargets (avoids many-to-many issues between daily dates and monthly targets)

## Key DAX Measures
- `Total Sales = SUM(FactSales[Sales])`
- `Total Cost = SUM(FactSales[Cost])`
- `Total Profit = [Total Sales] - [Total Cost]`
- `Total Orders = DISTINCTCOUNT(FactSales[Order_ID])`
- `Total Quantity = SUM(FactSales[Quantity])`
- `Profit Margin % = DIVIDE([Total Profit], [Total Sales], 0)`
- `Average Order Value = DIVIDE([Total Sales], [Total Orders], 0)`
- `Sales LY = CALCULATE([Total Sales], SAMEPERIODLASTYEAR(DimDate[Date]))`
- `Sales YoY % = DIVIDE([Total Sales] - [Sales LY], [Sales LY], 0)`
- `YTD Sales = TOTALYTD([Total Sales], DimDate[Date])`
- `Target Sales = SUM(MonthlyTargets[Target_Amount])`
- `Target Achievement %
