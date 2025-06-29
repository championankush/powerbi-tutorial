# Advanced Power Query Techniques 🔧

## Introduction

This tutorial covers advanced Power Query techniques using the M language, complex data transformations, and real-world scenarios that will help you master data preparation in PowerBI.

## Table of Contents
1. [M Language Fundamentals](#m-language-fundamentals)
2. [Advanced Data Transformations](#advanced-data-transformations)
3. [Error Handling](#error-handling)
4. [Performance Optimization](#performance-optimization)
5. [Custom Functions](#custom-functions)
6. [Data Source Management](#data-source-management)
7. [Complex Scenarios](#complex-scenarios)
8. [Best Practices](#best-practices)
9. [Troubleshooting](#troubleshooting)
10. [Real-World Examples](#real-world-examples)

## M Language Fundamentals

### What is M Language?
M is the functional programming language used by Power Query for data transformation. It's case-sensitive and uses a declarative approach.

### Basic M Syntax
```m
// Basic query structure
let
    Source = Excel.Workbook(File.Contents("C:\Data\Sales.xlsx"), null, true),
    Sales_Sheet = Source{[Item="Sales",Kind="Sheet"]}[Data],
    #"Promoted Headers" = Table.PromoteHeaders(Sales_Sheet, [PromoteAllScalars=true])
in
    #"Promoted Headers"
```

### Data Types in M
- **Text**: `"Hello World"`
- **Number**: `42` or `3.14`
- **Logical**: `true` or `false`
- **Date**: `#date(2023, 1, 1)`
- **Time**: `#time(14, 30, 0)`
- **DateTime**: `#datetime(2023, 1, 1, 14, 30, 0)`
- **List**: `{1, 2, 3, 4, 5}`
- **Record**: `[Name="John", Age=30]`
- **Table**: `#table({"Name", "Age"}, {{"John", 30}, {"Jane", 25}})`

## Advanced Data Transformations

### 1. Conditional Logic
```m
// Using if-then-else
= if [Sales] > 1000 then "High" else "Low"

// Using switch statement
= switch true
    [Sales] > 1000 => "High",
    [Sales] > 500 => "Medium",
    otherwise => "Low"
```

### 2. List Operations
```m
// Create list of numbers
= {1..10}

// List comprehension
= List.Transform({1..5}, each _ * 2)

// Filter list
= List.Select({1..10}, each _ > 5)

// Combine lists
= List.Combine({{1,2}, {3,4}, {5,6}})
```

### 3. Record Operations
```m
// Create record
= [Name="John", Age=30, City="New York"]

// Access record fields
= [Name="John", Age=30][Name]

// Merge records
= Record.Combine([Name="John"], [Age=30])
```

### 4. Table Operations
```m
// Add custom column with complex logic
= Table.AddColumn(Sales, "Sales Category", each
    if [Amount] > 1000 then "High Value"
    else if [Amount] > 500 then "Medium Value"
    else "Low Value"
)

// Group and aggregate
= Table.Group(Sales, {"Region"}, {
    {"Total Sales", each List.Sum([Amount]), type number},
    {"Average Sales", each List.Average([Amount]), type number},
    {"Sales Count", each Table.RowCount(_), type number}
})
```

## Error Handling

### 1. Try-Otherwise
```m
// Basic error handling
= try [Amount] / [Quantity] otherwise 0

// With custom error message
= try [Amount] / [Quantity] otherwise error "Division by zero"
```

### 2. Error Handling in Functions
```m
// Custom function with error handling
let
    SafeDivide = (numerator, denominator) =>
        try numerator / denominator otherwise 0
in
    SafeDivide
```

### 3. Replace Errors
```m
// Replace all errors in a column
= Table.ReplaceErrorValues(Sales, {{"Amount", 0}})

// Replace specific errors
= Table.ReplaceErrorValues(Sales, {
    {"Amount", 0},
    {"Quantity", 1}
})
```

## Performance Optimization

### 1. Query Folding
Query folding pushes transformations back to the data source, improving performance.

```m
// Good - will fold to SQL
= Table.SelectRows(Sales, each [Amount] > 1000)

// Bad - won't fold
= Table.AddColumn(Sales, "Custom", each Text.Upper([ProductName]))
```

### 2. Reduce Data Early
```m
// Filter data as early as possible
let
    Source = Sql.Database("Server", "Database"),
    Sales = Source{[Schema="dbo",Item="Sales"]}[Data],
    FilteredSales = Table.SelectRows(Sales, each [Date] >= #date(2023, 1, 1)),
    // Continue with other transformations...
in
    FilteredSales
```

### 3. Use Appropriate Data Types
```m
// Set correct data types
= Table.TransformColumnTypes(Sales, {
    {"Amount", type currency},
    {"Date", type date},
    {"ProductID", type text}
})
```

## Custom Functions

### 1. Creating Functions
```m
// Simple function
let
    AddPrefix = (text, prefix) => prefix & text
in
    AddPrefix

// Function with multiple parameters
let
    CalculateDiscount = (amount, discountRate) =>
        amount * (1 - discountRate / 100)
in
    CalculateDiscount
```

### 2. Function with Error Handling
```m
let
    SafeTextExtract = (text, start, length) =>
        try Text.Middle(text, start, length)
        otherwise ""
in
    SafeTextExtract
```

### 3. Invoking Functions
```m
// Call custom function
= Table.AddColumn(Sales, "Discounted Amount", 
    each CalculateDiscount([Amount], 10))
```

## Data Source Management

### 1. Multiple Data Sources
```m
let
    // SQL Server data
    SqlSource = Sql.Database("Server", "Database"),
    Sales = SqlSource{[Schema="dbo",Item="Sales"]}[Data],
    
    // Excel file
    ExcelSource = Excel.Workbook(File.Contents("C:\Data\Products.xlsx")),
    Products = ExcelSource{[Item="Products",Kind="Sheet"]}[Data],
    
    // Web API
    WebSource = Json.Document(Web.Contents("https://api.example.com/data")),
    
    // Combine data sources
    CombinedData = Table.NestedJoin(Sales, "ProductID", Products, "ProductID", "Products", JoinKind.LeftOuter)
in
    CombinedData
```

### 2. Parameterized Queries
```m
// Create parameters
let
    StartDate = #date(2023, 1, 1),
    EndDate = #date(2023, 12, 31),
    
    Source = Sql.Database("Server", "Database"),
    Sales = Source{[Schema="dbo",Item="Sales"]}[Data],
    FilteredSales = Table.SelectRows(Sales, each [Date] >= StartDate and [Date] <= EndDate)
in
    FilteredSales
```

## Complex Scenarios

### 1. Pivot and Unpivot
```m
// Pivot data
= Table.Pivot(Sales, List.Distinct(Sales[Month]), "Month", "Amount")

// Unpivot data
= Table.Unpivot(Sales, {"Jan", "Feb", "Mar"}, "Month", "Amount")
```

### 2. Merge Queries
```m
// Inner join
= Table.NestedJoin(Sales, "ProductID", Products, "ProductID", "Products", JoinKind.Inner)

// Left outer join
= Table.NestedJoin(Sales, "ProductID", Products, "ProductID", "Products", JoinKind.LeftOuter)

// Full outer join
= Table.NestedJoin(Sales, "ProductID", Products, "ProductID", "Products", JoinKind.FullOuter)
```

### 3. Conditional Column Merging
```m
// Merge based on conditions
= Table.AddColumn(Sales, "Product Info", each
    if [ProductID] = null then null
    else Table.SelectRows(Products, each [ProductID] = [ProductID]){0}
)
```

### 4. Dynamic Column Selection
```m
// Select columns dynamically
= Table.SelectColumns(Sales, 
    List.Select(Table.ColumnNames(Sales), each Text.Contains(_, "Sales"))
)
```

## Best Practices

### 1. Query Structure
- Use meaningful step names
- Keep queries modular
- Document complex transformations
- Use consistent naming conventions

### 2. Performance
- Enable query folding when possible
- Filter data early in the process
- Use appropriate data types
- Avoid unnecessary transformations

### 3. Error Handling
- Always handle potential errors
- Provide meaningful error messages
- Use try-otherwise for calculations
- Validate data quality

### 4. Maintainability
- Create reusable functions
- Use parameters for flexibility
- Document business logic
- Test transformations thoroughly

## Troubleshooting

### Common Issues

#### 1. Query Folding Issues
**Problem**: Transformations not folding to data source
**Solution**: 
- Check data source compatibility
- Use supported functions
- Monitor query performance

#### 2. Memory Issues
**Problem**: Large datasets causing memory problems
**Solution**:
- Filter data early
- Use incremental refresh
- Optimize data types

#### 3. Error Handling
**Problem**: Unexpected errors in transformations
**Solution**:
- Add try-otherwise blocks
- Validate data quality
- Use error replacement functions

### Debugging Techniques
```m
// Add debugging steps
= Table.AddColumn(Sales, "Debug", each 
    Text.Combine({Text.From([Amount]), " - ", Text.From([Quantity])})
)

// Check data types
= Table.Schema(Sales)

// Validate data
= Table.SelectRows(Sales, each [Amount] < 0)
```

## Real-World Examples

### 1. Sales Data Processing
```m
let
    Source = Excel.Workbook(File.Contents("C:\Data\Sales.xlsx")),
    Sales_Sheet = Source{[Item="Sales",Kind="Sheet"]}[Data],
    #"Promoted Headers" = Table.PromoteHeaders(Sales_Sheet),
    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers", {
        {"Date", type date},
        {"Amount", type currency},
        {"ProductID", type text}
    }),
    #"Filtered Rows" = Table.SelectRows(#"Changed Type", each [Amount] > 0),
    #"Added Custom" = Table.AddColumn(#"Filtered Rows", "Month", each Date.Month([Date])),
    #"Added Custom1" = Table.AddColumn(#"Added Custom", "Year", each Date.Year([Date])),
    #"Grouped Rows" = Table.Group(#"Added Custom1", {"Year", "Month"}, {
        {"Total Sales", each List.Sum([Amount]), type currency},
        {"Sales Count", each Table.RowCount(_), type number}
    })
in
    #"Grouped Rows"
```

### 2. Customer Data Cleansing
```m
let
    Source = Csv.Document(File.Contents("C:\Data\Customers.csv")),
    #"Promoted Headers" = Table.PromoteHeaders(Source),
    #"Cleaned Names" = Table.TransformColumns(#"Promoted Headers", {
        {"FirstName", each Text.Proper(_)},
        {"LastName", each Text.Proper(_)}
    }),
    #"Removed Duplicates" = Table.Distinct(#"Cleaned Names"),
    #"Filtered Valid Emails" = Table.SelectRows(#"Removed Duplicates", 
        each Text.Contains([Email], "@"))
in
    #"Filtered Valid Emails"
```

### 3. Financial Data Aggregation
```m
let
    Source = Sql.Database("FinanceDB", "Transactions"),
    Transactions = Source{[Schema="dbo",Item="Transactions"]}[Data],
    #"Filtered Current Year" = Table.SelectRows(Transactions, 
        each Date.Year([TransactionDate]) = Date.Year(DateTime.LocalNow())),
    #"Added Quarter" = Table.AddColumn(#"Filtered Current Year", "Quarter", 
        each "Q" & Text.From(Date.QuarterOfYear([TransactionDate]))),
    #"Grouped by Quarter" = Table.Group(#"Added Quarter", {"Quarter"}, {
        {"Total Revenue", each List.Sum([Amount]), type currency},
        {"Transaction Count", each Table.RowCount(_), type number},
        {"Average Transaction", each List.Average([Amount]), type currency}
    })
in
    #"Grouped by Quarter"
```

## Summary

Advanced Power Query techniques enable you to:
- Handle complex data transformations
- Create reusable functions
- Optimize query performance
- Handle errors gracefully
- Process multiple data sources
- Implement business logic

## Next Steps
- Practice with real datasets
- Learn more M language functions
- Explore Power Query community
- Experiment with custom functions
- Study performance optimization techniques

---

**Master Power Query to transform any data into analysis-ready format!** 🔧 