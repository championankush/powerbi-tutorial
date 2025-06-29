# DAX Fundamentals 📊

## What is DAX?

**DAX (Data Analysis Expressions)** is the formula language used in PowerBI, SQL Server Analysis Services, and Power Pivot in Excel. It's designed to work with tabular data models and provides a powerful way to create calculated columns, measures, and calculated tables.

## DAX vs Excel Formulas

| Feature | Excel | DAX |
|---------|-------|-----|
| **Purpose** | Cell-based calculations | Column-based calculations |
| **Context** | Single cell | Entire column/table |
| **Functions** | 400+ functions | 200+ functions |
| **Performance** | Slower with large data | Optimized for large datasets |

## DAX Syntax Basics

### Basic Formula Structure
```dax
MeasureName = CALCULATE(
    SUM(TableName[ColumnName]),
    FilterCondition
)
```

### Key Components:
- **Measure Name**: What you're calculating
- **Function**: The calculation to perform
- **Table/Column Reference**: `TableName[ColumnName]`
- **Filters**: Optional conditions

## DAX Data Types

| Type | Description | Example |
|------|-------------|---------|
| **Whole Number** | Integer values | 1, 2, 3, -1 |
| **Decimal Number** | Decimal values | 1.5, 2.75, -0.5 |
| **Fixed Decimal Number** | Currency values | $1.99, €2.50 |
| **Date/Time** | Date and time | 2024-01-15, 14:30:00 |
| **Boolean** | True/False | TRUE, FALSE |
| **Text** | String values | "Hello", "Product A" |
| **Binary** | Binary data | Images, files |

## Essential DAX Functions

### 1. Aggregation Functions

#### SUM
```dax
Total Sales = SUM(Sales[Amount])
```

#### AVERAGE
```dax
Average Price = AVERAGE(Products[Price])
```

#### COUNT
```dax
Number of Orders = COUNT(Sales[OrderID])
```

#### COUNTROWS
```dax
Total Products = COUNTROWS(Products)
```

#### MIN/MAX
```dax
Lowest Price = MIN(Products[Price])
Highest Price = MAX(Products[Price])
```

### 2. Text Functions

#### CONCATENATE
```dax
Full Name = CONCATENATE(Customers[FirstName], " ", Customers[LastName])
```

#### LEFT/RIGHT
```dax
First 3 Letters = LEFT(Products[ProductName], 3)
Last 4 Digits = RIGHT(Orders[OrderNumber], 4)
```

#### UPPER/LOWER
```dax
Upper Case = UPPER(Products[Category])
Lower Case = LOWER(Products[Category])
```

### 3. Date Functions

#### TODAY/NOW
```dax
Current Date = TODAY()
Current DateTime = NOW()
```

#### YEAR/MONTH/DAY
```dax
Order Year = YEAR(Orders[OrderDate])
Order Month = MONTH(Orders[OrderDate])
Order Day = DAY(Orders[OrderDate])
```

#### DATE
```dax
Custom Date = DATE(2024, 1, 15)
```

### 4. Logical Functions

#### IF
```dax
Sales Status = IF(Sales[Amount] > 1000, "High", "Low")
```

#### SWITCH
```dax
Category Group = SWITCH(
    Products[Category],
    "Electronics", "Tech",
    "Clothing", "Fashion",
    "Books", "Education",
    "Other"
)
```

#### AND/OR
```dax
Premium Customer = AND(
    Customers[TotalSpent] > 1000,
    Customers[OrderCount] > 5
)
```

## Calculated Columns vs Measures

### Calculated Columns
- **Created in**: Data view
- **Storage**: Physical data in the model
- **Use case**: Row-level calculations
- **Performance**: Slower with large datasets

```dax
// Calculated Column Example
Sales[Profit] = Sales[Revenue] - Sales[Cost]
```

### Measures
- **Created in**: Report view
- **Storage**: Calculated on-demand
- **Use case**: Aggregations and KPIs
- **Performance**: Optimized for large datasets

```dax
// Measure Example
Total Revenue = SUM(Sales[Revenue])
```

## Filter Context and Row Context

### Filter Context
- Applied by visualizations, slicers, and filters
- Determines which rows are included in calculations

```dax
// This measure respects filter context
Sales by Category = SUM(Sales[Amount])
```

### Row Context
- Applied when iterating through rows
- Used in calculated columns and iterator functions

```dax
// Calculated column with row context
Sales[Profit Margin] = DIVIDE(Sales[Profit], Sales[Revenue], 0)
```

## CALCULATE Function

The `CALCULATE` function is the most important DAX function. It modifies the filter context.

### Basic Syntax
```dax
CALCULATE(Expression, Filter1, Filter2, ...)
```

### Examples

#### Simple Filter
```dax
High Value Sales = CALCULATE(
    SUM(Sales[Amount]),
    Sales[Amount] > 1000
)
```

#### Multiple Filters
```dax
Electronics Sales 2024 = CALCULATE(
    SUM(Sales[Amount]),
    Products[Category] = "Electronics",
    YEAR(Sales[Date]) = 2024
)
```

#### Remove Filters
```dax
Total Sales All Time = CALCULATE(
    SUM(Sales[Amount]),
    ALL(Sales)
)
```

## Time Intelligence Functions

### SAMEPERIODLASTYEAR
```dax
Sales LY = CALCULATE(
    SUM(Sales[Amount]),
    SAMEPERIODLASTYEAR(Sales[Date])
)
```

### PREVIOUSMONTH
```dax
Sales Previous Month = CALCULATE(
    SUM(Sales[Amount]),
    PREVIOUSMONTH(Sales[Date])
)
```

### DATESYTD
```dax
Sales YTD = CALCULATE(
    SUM(Sales[Amount]),
    DATESYTD(Sales[Date])
)
```

## Iterator Functions

### SUMX
```dax
Total Profit = SUMX(
    Sales,
    Sales[Quantity] * (Sales[UnitPrice] - Sales[UnitCost])
)
```

### AVERAGEX
```dax
Average Order Value = AVERAGEX(
    Orders,
    Orders[TotalAmount]
)
```

### COUNTX
```dax
Products with Sales = COUNTX(
    Products,
    IF(CALCULATE(COUNTROWS(Sales)) > 0, 1, 0)
)
```

## Best Practices

### 1. Naming Conventions
```dax
// Good naming
Total Sales Amount = SUM(Sales[Amount])
Sales Amount LY = CALCULATE(SUM(Sales[Amount]), SAMEPERIODLASTYEAR(Sales[Date]))

// Avoid
Sales = SUM(Sales[Amount])
```

### 2. Performance Tips
- Use measures instead of calculated columns when possible
- Avoid complex calculations in calculated columns
- Use variables for complex calculations
- Test performance with large datasets

### 3. Error Handling
```dax
Safe Division = DIVIDE(Numerator, Denominator, 0)
Safe Text = IF(ISBLANK(TextColumn), "N/A", TextColumn)
```

## Common DAX Patterns

### 1. Percentage of Total
```dax
Sales % of Total = DIVIDE(
    SUM(Sales[Amount]),
    CALCULATE(SUM(Sales[Amount]), ALL(Sales)),
    0
)
```

### 2. Running Total
```dax
Running Total = CALCULATE(
    SUM(Sales[Amount]),
    FILTER(
        ALL(Sales),
        Sales[Date] <= MAX(Sales[Date])
    )
)
```

### 3. Top N Filter
```dax
Top 5 Products = CALCULATE(
    SUM(Sales[Amount]),
    TOPN(5, Products, SUM(Sales[Amount]))
)
```

## Practice Exercises

### Exercise 1: Basic Calculations
Create these measures:
1. Total Revenue
2. Average Order Value
3. Number of Customers
4. Profit Margin

### Exercise 2: Time Intelligence
Create these measures:
1. Sales This Year
2. Sales Last Year
3. Year-over-Year Growth
4. Month-to-Date Sales

### Exercise 3: Conditional Logic
Create these measures:
1. High Value Sales (>$1000)
2. Sales by Region (North/South/East/West)
3. Premium Customer Sales

## Next Steps

Ready to advance? Continue with:
1. [Advanced DAX Functions](02-advanced-dax.md)
2. [DAX Performance Optimization](03-dax-performance.md)
3. [Real-world DAX Examples](04-real-world-examples.md)

## Resources

- [DAX Reference](https://docs.microsoft.com/en-us/dax/)
- [DAX Patterns](https://www.daxpatterns.com/)
- [PowerBI DAX Guide](https://powerbi.microsoft.com/en-us/blog/tag/dax/)

---

**Master the fundamentals first, then advance to complex scenarios!** 🚀 