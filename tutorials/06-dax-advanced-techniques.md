# Advanced DAX Techniques 🧮

## Introduction

This tutorial covers advanced DAX (Data Analysis Expressions) techniques for creating complex calculations, time intelligence functions, and optimized measures in PowerBI.

## Table of Contents
1. [DAX Fundamentals Review](#dax-fundamentals-review)
2. [Advanced Filter Context](#advanced-filter-context)
3. [Time Intelligence Functions](#time-intelligence-functions)
4. [Complex Calculations](#complex-calculations)
5. [Row Context vs Filter Context](#row-context-vs-filter-context)
6. [Variables and Performance](#variables-and-performance)
7. [Advanced Functions](#advanced-functions)
8. [Optimization Techniques](#optimization-techniques)
9. [Real-World Scenarios](#real-world-scenarios)
10. [Best Practices](#best-practices)

## DAX Fundamentals Review

### Basic DAX Syntax
```dax
// Simple measure
Total Sales = SUM(Sales[Amount])

// Calculated column
Full Name = Customers[FirstName] & " " & Customers[LastName]

// Table function
Top 10 Products = TOPN(10, Products, [Total Sales])
```

### Context Types
- **Row Context**: Current row in a table
- **Filter Context**: Filters applied to calculations
- **Query Context**: Overall query environment

## Advanced Filter Context

### 1. CALCULATE Function
The most important DAX function for modifying filter context.

```dax
// Basic CALCULATE
Sales in North = CALCULATE(SUM(Sales[Amount]), Customers[Region] = "North")

// Multiple filters
High Value Sales = CALCULATE(
    SUM(Sales[Amount]),
    Sales[Amount] > 1000,
    Customers[Segment] = "Premium"
)
```

### 2. Filter Functions

#### ALL Function
```dax
// Remove all filters
Total Sales All = CALCULATE(SUM(Sales[Amount]), ALL(Sales))

// Remove specific filters
Sales by Region = CALCULATE(
    SUM(Sales[Amount]),
    ALL(Customers[City], Customers[State])
)
```

#### ALLEXCEPT Function
```dax
// Keep specific filters, remove others
Sales by Product = CALCULATE(
    SUM(Sales[Amount]),
    ALLEXCEPT(Products, Products[Category])
)
```

#### ALLSELECTED Function
```dax
// Respect user selections
Selected Sales = CALCULATE(
    SUM(Sales[Amount]),
    ALLSELECTED()
)
```

### 3. Advanced Filtering
```dax
// Complex filter conditions
Premium Sales = CALCULATE(
    SUM(Sales[Amount]),
    Customers[Segment] = "Premium",
    Sales[Amount] > 500,
    Products[Category] IN {"Electronics", "Clothing"}
)

// Dynamic filtering
Dynamic Sales = CALCULATE(
    SUM(Sales[Amount]),
    FILTER(Sales, Sales[Amount] > AVERAGE(Sales[Amount]))
)
```

## Time Intelligence Functions

### 1. Basic Time Intelligence
```dax
// Previous period
Previous Month Sales = CALCULATE(
    SUM(Sales[Amount]),
    PREVIOUSMONTH(Sales[Date])
)

// Next period
Next Month Sales = CALCULATE(
    SUM(Sales[Amount]),
    NEXTMONTH(Sales[Date])
)

// Same period last year
Sales LY = CALCULATE(
    SUM(Sales[Amount]),
    SAMEPERIODLASTYEAR(Sales[Date])
)
```

### 2. Advanced Time Intelligence
```dax
// Year-to-date
YTD Sales = TOTALYTD(SUM(Sales[Amount]), Sales[Date])

// Quarter-to-date
QTD Sales = TOTALQTD(SUM(Sales[Amount]), Sales[Date])

// Month-to-date
MTD Sales = TOTALMTD(SUM(Sales[Amount]), Sales[Date])

// Rolling 12 months
Rolling 12M Sales = CALCULATE(
    SUM(Sales[Amount]),
    DATESINPERIOD(Sales[Date], LASTDATE(Sales[Date]), -12, MONTH)
)
```

### 3. Custom Time Periods
```dax
// Custom date range
Last 30 Days = CALCULATE(
    SUM(Sales[Amount]),
    DATESINPERIOD(Sales[Date], LASTDATE(Sales[Date]), -30, DAY)
)

// Fiscal year
Fiscal YTD = CALCULATE(
    SUM(Sales[Amount]),
    DATESYTD(Sales[Date], "7-1")
)
```

## Complex Calculations

### 1. Running Totals
```dax
// Running total
Running Total = CALCULATE(
    SUM(Sales[Amount]),
    FILTER(
        ALL(Sales[Date]),
        Sales[Date] <= MAX(Sales[Date])
    )
)

// Running total by category
Running Total by Category = CALCULATE(
    SUM(Sales[Amount]),
    FILTER(
        ALL(Sales),
        Sales[Date] <= MAX(Sales[Date]) &&
        Sales[ProductID] = MAX(Sales[ProductID])
    )
)
```

### 2. Percentages and Ratios
```dax
// Percentage of total
% of Total = DIVIDE(SUM(Sales[Amount]), CALCULATE(SUM(Sales[Amount]), ALL()), 0)

// Percentage by category
% by Category = DIVIDE(
    SUM(Sales[Amount]),
    CALCULATE(SUM(Sales[Amount]), ALL(Products[Category])),
    0
)

// Market share
Market Share = DIVIDE(
    SUM(Sales[Amount]),
    CALCULATE(SUM(Sales[Amount]), ALL(Customers[Region])),
    0
)
```

### 3. Ranking and Percentiles
```dax
// Rank by sales
Sales Rank = RANKX(ALL(Products), [Total Sales],, DESC)

// Top N products
Top 5 Products = CALCULATE(
    COUNTROWS(Products),
    TOPN(5, ALL(Products), [Total Sales])
)

// Percentile
Sales Percentile = PERCENTILE.INC(ALL(Sales[Amount]), 0.75)
```

## Row Context vs Filter Context

### Understanding Context
```dax
// Row context example (calculated column)
Row Context Example = Sales[Amount] * 1.1

// Filter context example (measure)
Filter Context Example = SUM(Sales[Amount])
```

### Context Transition
```dax
// Using EARLIER function
Running Total with Earlier = CALCULATE(
    SUM(Sales[Amount]),
    FILTER(
        ALL(Sales),
        Sales[Date] <= EARLIER(Sales[Date])
    )
)
```

### Iterator Functions
```dax
// SUMX example
Total Sales with Tax = SUMX(Sales, Sales[Amount] * 1.1)

// AVERAGEX example
Average Order Value = AVERAGEX(Sales, Sales[Amount])

// COUNTX example
Orders with High Value = COUNTX(Sales, IF(Sales[Amount] > 1000, 1, 0))
```

## Variables and Performance

### 1. Using Variables
```dax
// Simple variable
Sales with Variables = 
VAR TotalAmount = SUM(Sales[Amount])
VAR AverageAmount = AVERAGE(Sales[Amount])
RETURN
IF(TotalAmount > AverageAmount, "Above Average", "Below Average")

// Complex calculation with variables
Complex Calculation = 
VAR CurrentSales = SUM(Sales[Amount])
VAR PreviousSales = CALCULATE(SUM(Sales[Amount]), PREVIOUSMONTH(Sales[Date]))
VAR Growth = DIVIDE(CurrentSales - PreviousSales, PreviousSales, 0)
VAR GrowthCategory = 
    SWITCH(
        TRUE(),
        Growth > 0.1, "High Growth",
        Growth > 0, "Positive Growth",
        "Declining"
    )
RETURN
GrowthCategory
```

### 2. Performance Optimization
```dax
// Optimized measure
Optimized Sales = 
VAR CurrentContext = ALLSELECTED()
VAR FilteredSales = CALCULATE(SUM(Sales[Amount]), CurrentContext)
RETURN
FilteredSales

// Avoid repeated calculations
Efficient Calculation = 
VAR TotalSales = SUM(Sales[Amount])
VAR TotalOrders = COUNTROWS(Sales)
VAR AverageOrder = DIVIDE(TotalSales, TotalOrders, 0)
RETURN
AverageOrder
```

## Advanced Functions

### 1. Table Functions
```dax
// SUMMARIZE
Sales Summary = SUMMARIZE(
    Sales,
    Products[Category],
    "Total Sales", SUM(Sales[Amount]),
    "Order Count", COUNTROWS(Sales)
)

// ADDCOLUMNS
Sales with Metrics = ADDCOLUMNS(
    Products,
    "Total Sales", [Total Sales],
    "Average Price", AVERAGE(Sales[Amount])
)

// CROSSJOIN
Product Customer Matrix = CROSSJOIN(Products, Customers)
```

### 2. Text Functions
```dax
// CONCATENATEX
Product List = CONCATENATEX(Products, Products[Name], ", ")

// SUBSTITUTE
Clean Product Name = SUBSTITUTE(Products[Name], " - ", " ")

// LEFT and RIGHT
Product Code = LEFT(Products[ProductID], 3)
```

### 3. Logical Functions
```dax
// SWITCH with TRUE
Performance Category = 
SWITCH(
    TRUE(),
    [Sales Amount] > 10000, "Excellent",
    [Sales Amount] > 5000, "Good",
    [Sales Amount] > 1000, "Average",
    "Below Average"
)

// IF with multiple conditions
Complex Logic = 
IF(
    AND([Sales Amount] > 5000, [Customer Count] > 100),
    "High Performance",
    IF([Sales Amount] > 2000, "Medium Performance", "Low Performance")
)
```

## Optimization Techniques

### 1. Measure Optimization
```dax
// Use variables to avoid repeated calculations
Optimized Measure = 
VAR TotalSales = SUM(Sales[Amount])
VAR TotalCost = SUM(Sales[Cost])
VAR Profit = TotalSales - TotalCost
VAR ProfitMargin = DIVIDE(Profit, TotalSales, 0)
RETURN
ProfitMargin
```

### 2. Filter Optimization
```dax
// Use ALL instead of ALLEXCEPT when possible
Optimized Filter = CALCULATE(
    SUM(Sales[Amount]),
    ALL(Customers[City], Customers[State])
)
```

### 3. Context Optimization
```dax
// Minimize context transitions
Efficient Context = 
VAR CurrentSales = SUM(Sales[Amount])
VAR PreviousSales = CALCULATE(CurrentSales, PREVIOUSMONTH(Sales[Date]))
RETURN
DIVIDE(CurrentSales - PreviousSales, PreviousSales, 0)
```

## Real-World Scenarios

### 1. Sales Analytics
```dax
// Sales growth by period
Sales Growth = 
VAR CurrentPeriod = [Total Sales]
VAR PreviousPeriod = CALCULATE([Total Sales], PREVIOUSMONTH(Sales[Date]))
VAR Growth = DIVIDE(CurrentPeriod - PreviousPeriod, PreviousPeriod, 0)
RETURN
Growth

// Customer lifetime value
Customer LTV = 
VAR CustomerSales = SUM(Sales[Amount])
VAR CustomerOrders = COUNTROWS(Sales)
VAR AverageOrder = DIVIDE(CustomerSales, CustomerOrders, 0)
VAR RetentionRate = 0.8
VAR LTV = AverageOrder * RetentionRate / (1 - RetentionRate)
RETURN
LTV
```

### 2. Inventory Management
```dax
// Inventory turnover
Inventory Turnover = 
VAR TotalSales = SUM(Sales[Quantity])
VAR AverageInventory = AVERAGE(Products[StockLevel])
VAR Turnover = DIVIDE(TotalSales, AverageInventory, 0)
RETURN
Turnover

// Days of inventory
Days of Inventory = 
VAR AverageInventory = AVERAGE(Products[StockLevel])
VAR DailySales = DIVIDE(SUM(Sales[Quantity]), 365, 0)
VAR Days = DIVIDE(AverageInventory, DailySales, 0)
RETURN
Days
```

### 3. Financial Analysis
```dax
// Profit margin by product
Profit Margin = 
VAR Revenue = SUM(Sales[Amount])
VAR Cost = SUM(Sales[Cost])
VAR Profit = Revenue - Cost
VAR Margin = DIVIDE(Profit, Revenue, 0)
RETURN
Margin

// Return on investment
ROI = 
VAR Investment = SUM(Products[Cost])
VAR Returns = SUM(Sales[Amount])
VAR ROI = DIVIDE(Returns - Investment, Investment, 0)
RETURN
ROI
```

## Best Practices

### 1. Naming Conventions
- Use descriptive names for measures
- Prefix measures with calculation type
- Use consistent formatting

### 2. Performance
- Use variables to avoid repeated calculations
- Minimize context transitions
- Use appropriate aggregation functions

### 3. Maintainability
- Document complex calculations
- Use consistent patterns
- Test measures thoroughly

### 4. Error Handling
```dax
// Safe division
Safe Division = DIVIDE(Numerator, Denominator, 0)

// Error handling with IF
Safe Calculation = 
IF(
    ISBLANK([Denominator]) || [Denominator] = 0,
    0,
    [Numerator] / [Denominator]
)
```

## Troubleshooting

### Common Issues
1. **Circular dependencies**: Check for self-referencing measures
2. **Performance problems**: Use variables and optimize filters
3. **Incorrect results**: Verify filter context and relationships
4. **Blank values**: Handle nulls with IF or ISBLANK

### Debugging Techniques
```dax
// Debug measure
Debug Measure = 
VAR DebugValue = [Your Measure]
VAR DebugText = "Debug: " & FORMAT(DebugValue, "0.00")
RETURN
DebugText
```

## Summary

Advanced DAX techniques enable you to:
- Create complex calculations
- Implement time intelligence
- Optimize performance
- Handle real-world scenarios
- Build maintainable solutions

## Next Steps
- Practice with real datasets
- Learn more DAX functions
- Study performance optimization
- Join DAX community forums
- Experiment with complex scenarios

---

**Master advanced DAX to create powerful, efficient calculations in PowerBI!** 🧮 