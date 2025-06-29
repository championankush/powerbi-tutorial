# Advanced DAX Functions 🚀

## Introduction

Once you've mastered the fundamentals, it's time to explore advanced DAX techniques that will take your PowerBI reports to the next level. This guide covers complex functions, optimization techniques, and real-world patterns.

## Advanced Filter Functions

### ALL Function Family

#### ALL - Remove All Filters
```dax
Total Sales All Time = CALCULATE(
    SUM(Sales[Amount]),
    ALL(Sales)
)
```

#### ALLEXCEPT - Keep Specific Filters
```dax
Sales by Product = CALCULATE(
    SUM(Sales[Amount]),
    ALLEXCEPT(Products, Products[Category])
)
```

#### ALLSELECTED - Respect User Selections
```dax
Sales vs Selected = CALCULATE(
    SUM(Sales[Amount]),
    ALLSELECTED(Sales)
)
```

### FILTER Function

#### Complex Filtering
```dax
High Value Electronics = CALCULATE(
    SUM(Sales[Amount]),
    FILTER(
        Products,
        Products[Category] = "Electronics" && 
        Products[Price] > 500
    )
)
```

#### Multiple Table Filters
```dax
Premium Customer Sales = CALCULATE(
    SUM(Sales[Amount]),
    FILTER(
        Customers,
        Customers[TotalSpent] > 1000 && 
        Customers[OrderCount] > 5
    )
)
```

## Advanced Time Intelligence

### Custom Date Tables

#### Creating Date Dimensions
```dax
// Date Table Creation
DateTable = CALENDAR(
    DATE(2020, 1, 1),
    DATE(2024, 12, 31)
)
```

#### Adding Date Columns
```dax
// In DateTable
Year = YEAR([Date])
Month = MONTH([Date])
MonthName = FORMAT([Date], "mmmm")
Quarter = "Q" & ROUNDUP(MONTH([Date])/3, 0)
WeekDay = WEEKDAY([Date], 2)
```

### Advanced Time Calculations

#### Rolling Averages
```dax
3-Month Rolling Average = AVERAGEX(
    DATESINPERIOD(
        Sales[Date],
        LASTDATE(Sales[Date]),
        -3,
        MONTH
    ),
    SUM(Sales[Amount])
)
```

#### Year-to-Date vs Previous Year
```dax
YTD vs PY = CALCULATE(
    SUM(Sales[Amount]),
    DATESYTD(Sales[Date])
) - CALCULATE(
    SUM(Sales[Amount]),
    DATESYTD(SAMEPERIODLASTYEAR(Sales[Date]))
)
```

#### Month-over-Month Growth
```dax
MoM Growth % = DIVIDE(
    SUM(Sales[Amount]) - CALCULATE(
        SUM(Sales[Amount]),
        PREVIOUSMONTH(Sales[Date])
    ),
    CALCULATE(
        SUM(Sales[Amount]),
        PREVIOUSMONTH(Sales[Date])
    ),
    0
)
```

## Advanced Iterator Functions

### MAXX/MINX
```dax
Highest Single Sale = MAXX(
    Sales,
    Sales[Amount]
)

Lowest Product Price = MINX(
    Products,
    Products[Price]
)
```

### RANKX
```dax
Product Rank = RANKX(
    ALL(Products),
    SUM(Sales[Amount]),
    ,
    DESC
)
```

### TOPN with Dynamic N
```dax
Top N Products = CALCULATE(
    SUM(Sales[Amount]),
    TOPN(
        [Top N Value], // Parameter
        Products,
        SUM(Sales[Amount])
    )
)
```

## Variables in DAX

### Using VAR for Performance
```dax
Complex Calculation = 
VAR TotalSales = SUM(Sales[Amount])
VAR TotalCost = SUM(Sales[Cost])
VAR Profit = TotalSales - TotalCost
VAR ProfitMargin = DIVIDE(Profit, TotalSales, 0)
RETURN
    ProfitMargin
```

### Variables with Multiple Steps
```dax
Customer Segmentation = 
VAR CustomerSpend = SUM(Sales[Amount])
VAR AvgSpend = AVERAGEX(ALL(Customers), SUM(Sales[Amount]))
VAR Segment = SWITCH(
    TRUE(),
    CustomerSpend > AvgSpend * 2, "Premium",
    CustomerSpend > AvgSpend, "Regular",
    "Budget"
)
RETURN
    Segment
```

## Advanced CALCULATE Patterns

### CALCULATE with Multiple Filters
```dax
Multi-Condition Sales = CALCULATE(
    SUM(Sales[Amount]),
    Products[Category] = "Electronics",
    Sales[Date] >= DATE(2024, 1, 1),
    Sales[Date] <= DATE(2024, 12, 31),
    Customers[Region] IN {"North", "South"}
)
```

### CALCULATE with Table Filters
```dax
Sales in Selected Period = CALCULATE(
    SUM(Sales[Amount]),
    FILTER(
        ALL(Sales[Date]),
        Sales[Date] >= [Start Date] && 
        Sales[Date] <= [End Date]
    )
)
```

## Advanced Text Functions

### CONCATENATEX
```dax
Product List = CONCATENATEX(
    Products,
    Products[ProductName],
    ", ",
    Products[ProductName],
    ASC
)
```

### SUBSTITUTE and SEARCH
```dax
Clean Product Name = SUBSTITUTE(
    Products[ProductName],
    " - ",
    " "
)
```

### FORMAT Function
```dax
Formatted Amount = FORMAT(
    SUM(Sales[Amount]),
    "Currency"
)

Formatted Date = FORMAT(
    Sales[Date],
    "mmmm yyyy"
)
```

## Advanced Logical Functions

### SWITCH with TRUE()
```dax
Dynamic Category = SWITCH(
    TRUE(),
    SUM(Sales[Amount]) > 10000, "High",
    SUM(Sales[Amount]) > 5000, "Medium",
    SUM(Sales[Amount]) > 1000, "Low",
    "Very Low"
)
```

### Nested IF Statements
```dax
Complex Logic = IF(
    SUM(Sales[Amount]) > 10000,
    "Excellent",
    IF(
        SUM(Sales[Amount]) > 5000,
        "Good",
        IF(
            SUM(Sales[Amount]) > 1000,
            "Average",
            "Poor"
        )
    )
)
```

## Advanced Aggregation Patterns

### Weighted Averages
```dax
Weighted Average Price = DIVIDE(
    SUMX(
        Sales,
        Sales[Quantity] * RELATED(Products[Price])
    ),
    SUM(Sales[Quantity]),
    0
)
```

### Conditional Aggregations
```dax
High Value Sales Count = COUNTX(
    FILTER(
        Sales,
        Sales[Amount] > 1000
    ),
    Sales[OrderID]
)
```

## Advanced Relationship Functions

### USERELATIONSHIP
```dax
Sales by Delivery Date = CALCULATE(
    SUM(Sales[Amount]),
    USERELATIONSHIP(Sales[DeliveryDate], DateTable[Date])
)
```

### CROSSFILTER
```dax
Bidirectional Filter = CALCULATE(
    SUM(Sales[Amount]),
    CROSSFILTER(Sales[ProductID], Products[ProductID], BOTH)
)
```

## Performance Optimization Techniques

### Using Variables for Complex Calculations
```dax
Optimized Calculation = 
VAR BaseSales = SUM(Sales[Amount])
VAR ElectronicsSales = CALCULATE(
    BaseSales,
    Products[Category] = "Electronics"
)
VAR ElectronicsRatio = DIVIDE(ElectronicsSales, BaseSales, 0)
RETURN
    ElectronicsRatio
```

### Avoiding Multiple CALCULATE Calls
```dax
// Inefficient
Sales 2024 = CALCULATE(SUM(Sales[Amount]), YEAR(Sales[Date]) = 2024)
Sales 2023 = CALCULATE(SUM(Sales[Amount]), YEAR(Sales[Date]) = 2023)
Growth = Sales 2024 - Sales 2023

// Efficient
Growth = 
VAR Sales2024 = CALCULATE(SUM(Sales[Amount]), YEAR(Sales[Date]) = 2024)
VAR Sales2023 = CALCULATE(SUM(Sales[Amount]), YEAR(Sales[Date]) = 2023)
RETURN
    Sales2024 - Sales2023
```

## Advanced Error Handling

### Comprehensive Error Checking
```dax
Safe Calculation = 
VAR Numerator = SUM(Sales[Amount])
VAR Denominator = SUM(Sales[Quantity])
VAR Result = IF(
    ISBLANK(Numerator) || ISBLANK(Denominator) || Denominator = 0,
    BLANK(),
    DIVIDE(Numerator, Denominator, 0)
)
RETURN
    Result
```

### Data Quality Checks
```dax
Data Quality Score = 
VAR TotalRows = COUNTROWS(Sales)
VAR ValidRows = COUNTX(
    FILTER(
        Sales,
        NOT(ISBLANK(Sales[Amount])) && 
        Sales[Amount] > 0
    ),
    Sales[OrderID]
)
RETURN
    DIVIDE(ValidRows, TotalRows, 0)
```

## Real-World Advanced Patterns

### Customer Lifetime Value
```dax
Customer LTV = 
VAR CustomerSales = SUM(Sales[Amount])
VAR CustomerOrders = COUNTROWS(Sales)
VAR AvgOrderValue = DIVIDE(CustomerSales, CustomerOrders, 0)
VAR PurchaseFrequency = CustomerOrders / 12 // Assuming 12 months
VAR LTV = AvgOrderValue * PurchaseFrequency * 12
RETURN
    LTV
```

### Product Performance Score
```dax
Product Score = 
VAR SalesAmount = SUM(Sales[Amount])
VAR SalesCount = COUNTROWS(Sales)
VAR ProfitMargin = DIVIDE(
    SUM(Sales[Amount]) - SUM(Sales[Cost]),
    SUM(Sales[Amount]),
    0
)
VAR Score = (SalesAmount * 0.4) + (SalesCount * 0.3) + (ProfitMargin * 0.3)
RETURN
    Score
```

### Dynamic Top N Analysis
```dax
Dynamic Top N = 
VAR SelectedN = [Top N Parameter]
VAR TopProducts = TOPN(
    SelectedN,
    Products,
    SUM(Sales[Amount])
)
VAR TopSales = CALCULATE(
    SUM(Sales[Amount]),
    TopProducts
)
VAR TotalSales = SUM(Sales[Amount])
VAR TopPercentage = DIVIDE(TopSales, TotalSales, 0)
RETURN
    TopPercentage
```

## Advanced Visualization Measures

### KPI Status
```dax
KPI Status = 
VAR CurrentValue = SUM(Sales[Amount])
VAR TargetValue = [Sales Target]
VAR Status = SWITCH(
    TRUE(),
    CurrentValue >= TargetValue * 1.1, "Excellent",
    CurrentValue >= TargetValue, "On Target",
    CurrentValue >= TargetValue * 0.9, "At Risk",
    "Below Target"
)
RETURN
    Status
```

### Trend Indicators
```dax
Trend Indicator = 
VAR CurrentPeriod = SUM(Sales[Amount])
VAR PreviousPeriod = CALCULATE(
    SUM(Sales[Amount]),
    PREVIOUSMONTH(Sales[Date])
)
VAR Trend = CurrentPeriod - PreviousPeriod
VAR TrendText = IF(
    Trend > 0,
    "↗️ Up",
    IF(Trend < 0, "↘️ Down", "→ Flat")
)
RETURN
    TrendText
```

## Practice Exercises

### Exercise 1: Advanced Time Intelligence
Create these measures:
1. 12-Month Rolling Average
2. Quarter-over-Quarter Growth
3. Year-to-Date vs Previous Year-to-Date
4. Moving Average with Dynamic Periods

### Exercise 2: Complex Filtering
Create these measures:
1. Sales for Premium Customers in Electronics Category
2. Top 10% Products by Revenue
3. Sales Excluding Returns and Cancellations
4. Cross-Category Product Performance

### Exercise 3: Advanced Analytics
Create these measures:
1. Customer Segmentation (RFM Analysis)
2. Product Affinity Score
3. Seasonal Performance Index
4. Market Share by Category

## Next Steps

Ready for expert-level DAX? Continue with:
1. [DAX Performance Optimization](03-dax-performance.md)
2. [Real-world DAX Examples](04-real-world-examples.md)
3. [DAX Best Practices](05-dax-best-practices.md)

## Resources

- [DAX Patterns](https://www.daxpatterns.com/)
- [PowerBI DAX Reference](https://docs.microsoft.com/en-us/dax/)
- [DAX Studio](https://daxstudio.org/) - Performance testing tool

---

**Advanced DAX opens up unlimited possibilities for data analysis!** 🎯 