## Base Measures
Total Sales =
SUM ( FactSales[Sales] )

Total Profit =
SUM ( FactSales[Profit] )

Total Quantity =
SUM ( FactSales[Quantity] )

Profit Margin % =
DIVIDE ( [Total Profit], [Total Sales] )

## Order-Based Time Intelligence (Order Date Active)
Sales YTD =
TOTALYTD ( [Total Sales], DimDate[Date] )

Sales MTD =
TOTALMTD ( [Total Sales], DimDate[Date] )

Profit YTD =
TOTALYTD ( [Total Profit], DimDate[Date] )

Quantity YTD =
TOTALYTD ( [Total Quantity], DimDate[Date] )

## Year-Over-Year Analysis
Sales LY =
CALCULATE (
    [Total Sales],
    SAMEPERIODLASTYEAR ( DimDate[Date] )
)

Sales YoY =
[Total Sales] - [Sales LY]

Sales YoY % =
DIVIDE ( [Sales YoY], [Sales LY] )
Profit LY =
CALCULATE (
    [Total Profit],
    SAMEPERIODLASTYEAR ( DimDate[Date] )
)

Profit YoY =
[Total Profit] - [Profit LY]

Profit YoY % =
DIVIDE ( [Profit YoY], [Profit LY] )

## Tooltip logic:
Sales YoY Tooltip =
IF (
    ISBLANK ( [Sales LY] ),
    "YoY not available for first year of data",
    "Year-over-Year sales growth compared to previous year"
)

## Shipping & Operational Measures (Ship Date Relationship)
Orders Shipped =
DISTINCTCOUNT ( FactSales[OrderID] )

Total Sales (Ship Date) =
CALCULATE (
    [Total Sales],
    USERELATIONSHIP ( FactSales[ShipDate], DimDate[Date] )
)

Total Profit (Ship Date) =
CALCULATE (
    [Total Profit],
    USERELATIONSHIP ( FactSales[ShipDate], DimDate[Date] )
)

Total Quantity (Ship Date) =
CALCULATE (
    [Total Quantity],
    USERELATIONSHIP ( FactSales[ShipDate], DimDate[Date] )
)

Orders Shipped YTD =
CALCULATE (
    [Orders Shipped],
    USERELATIONSHIP ( FactSales[ShipDate], DimDate[Date] ),
    DATESYTD ( DimDate[Date] )
)

Total Quantity Shipped YTD =
CALCULATE (
    [Total Quantity],
    USERELATIONSHIP ( FactSales[ShipDate], DimDate[Date] ),
    DATESYTD ( DimDate[Date] )
)

## Operational Performance Metrics

Average Ship Days =
AVERAGE ( FactSales[Ship Days] )

On-Time Delivery (%) =
1 - [Late Orders (%)]
