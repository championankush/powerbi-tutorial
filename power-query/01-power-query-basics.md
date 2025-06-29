# Power Query Basics 🔧

## What is Power Query?

Power Query is a data connection technology that enables you to discover, connect, combine, and refine data across a wide variety of sources. It's the data transformation engine behind PowerBI, Excel, and other Microsoft products.

## Key Concepts

### Data Sources
Power Query can connect to:
- **Files**: Excel, CSV, JSON, XML, Text
- **Databases**: SQL Server, Oracle, MySQL, PostgreSQL
- **Online Services**: SharePoint, Dynamics 365, Salesforce
- **Web**: APIs, Web pages
- **Other**: ODBC, OData, Hadoop

### Data Transformation Steps
Each transformation creates a step in the query that can be:
- **Modified**: Change parameters
- **Reordered**: Move steps up/down
- **Deleted**: Remove unwanted steps
- **Renamed**: Give meaningful names

## Power Query Editor Interface

```
┌─────────────────────────────────────────────────────────────┐
│                    Power Query Editor                       │
├─────────────────────────────────────────────────────────────┤
│  Home  Transform  Add Column  View  Tools  Help            │
├─────────────────────────────────────────────────────────────┤
│  ┌─────────────────────────────────────────────────────┐   │
│  │                Applied Steps                        │   │
│  │  • Source                                         │   │
│  │  • Changed Type                                   │   │
│  │  • Removed Columns                                │   │
│  └─────────────────────────────────────────────────────┘   │
│  ┌─────────────────────────────────────────────────────┐   │
│  │                Data Preview                         │   │
│  │  ┌─────┬─────┬─────┬─────┐                         │   │
│  │  │ ID  │Name │Date │Value│                         │   │
│  │  ├─────┼─────┼─────┼─────┤                         │   │
│  │  │  1  │John │1/1  │ 100 │                         │   │
│  │  └─────┴─────┴─────┴─────┘                         │   │
│  └─────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

## Basic Transformations

### 1. Data Type Changes

#### Change Column Types
```m
// M Code for changing data types
= Table.TransformColumnTypes(
    Source,
    {
        {"Date", type date},
        {"Amount", type number},
        {"Category", type text}
    }
)
```

#### Common Data Types:
- `type text` - Text strings
- `type number` - Decimal numbers
- `type whole number` - Integers
- `type date` - Date values
- `type datetime` - Date and time
- `type logical` - True/False values

### 2. Column Operations

#### Remove Columns
```m
// Remove specific columns
= Table.RemoveColumns(
    Source,
    {"UnwantedColumn1", "UnwantedColumn2"}
)
```

#### Rename Columns
```m
// Rename columns
= Table.RenameColumns(
    Source,
    {
        {"OldName", "NewName"},
        {"Date", "OrderDate"}
    }
)
```

#### Add Custom Columns
```m
// Add calculated column
= Table.AddColumn(
    Source,
    "FullName",
    each [FirstName] & " " & [LastName]
)
```

### 3. Row Operations

#### Remove Rows
```m
// Remove top N rows
= Table.Skip(Source, 5)

// Remove bottom N rows
= Table.RemoveLastN(Source, 3)

// Remove rows with errors
= Table.RemoveRowsWithErrors(Source)
```

#### Filter Rows
```m
// Filter by condition
= Table.SelectRows(
    Source,
    each [Amount] > 1000
)

// Multiple conditions
= Table.SelectRows(
    Source,
    each [Amount] > 1000 and [Category] = "Electronics"
)
```

## Advanced Transformations

### 1. Text Transformations

#### Split Columns
```m
// Split by delimiter
= Table.SplitColumn(
    Source,
    "FullName",
    Splitter.SplitTextByDelimiter(" "),
    {"FirstName", "LastName"}
)
```

#### Replace Values
```m
// Replace text
= Table.ReplaceValue(
    Source,
    "OldValue",
    "NewValue",
    Replacer.ReplaceText,
    {"ColumnName"}
)
```

#### Extract Text
```m
// Extract first N characters
= Table.AddColumn(
    Source,
    "ShortName",
    each Text.Start([ProductName], 10)
)
```

### 2. Number Transformations

#### Mathematical Operations
```m
// Add percentage column
= Table.AddColumn(
    Source,
    "PriceWithTax",
    each [Price] * 1.1
)

// Round numbers
= Table.TransformColumns(
    Source,
    {{"Amount", each Number.Round(_, 2)}}
)
```

#### Conditional Calculations
```m
// Conditional logic
= Table.AddColumn(
    Source,
    "Discount",
    each if [Amount] > 1000 then [Amount] * 0.1 else 0
)
```

### 3. Date Transformations

#### Date Functions
```m
// Extract date parts
= Table.AddColumn(
    Source,
    "Year",
    each Date.Year([OrderDate])
)

// Add date calculations
= Table.AddColumn(
    Source,
    "DaysSinceOrder",
    each Duration.Days(DateTime.LocalNow() - [OrderDate])
)
```

#### Date Formatting
```m
// Format dates
= Table.TransformColumns(
    Source,
    {{"OrderDate", each Date.ToText(_, "yyyy-MM-dd")}}
)
```

## Merging and Appending Data

### 1. Merge Queries (Joins)

#### Inner Join
```m
// Merge two tables
= Table.NestedJoin(
    Orders,
    {"CustomerID"},
    Customers,
    {"CustomerID"},
    "CustomerData",
    JoinKind.Inner
)
```

#### Left Join
```m
// Keep all from left table
= Table.NestedJoin(
    Orders,
    {"CustomerID"},
    Customers,
    {"CustomerID"},
    "CustomerData",
    JoinKind.LeftOuter
)
```

#### Join Types:
- `JoinKind.Inner` - Only matching rows
- `JoinKind.LeftOuter` - All from left table
- `JoinKind.RightOuter` - All from right table
- `JoinKind.FullOuter` - All rows from both tables

### 2. Append Queries (Union)

#### Append Tables
```m
// Combine multiple tables
= Table.Combine({Table1, Table2, Table3})
```

#### Append with Transformations
```m
// Append with custom logic
= Table.Combine({
    Table.AddColumn(Table1, "Source", each "Table1"),
    Table.AddColumn(Table2, "Source", each "Table2")
})
```

## Pivoting and Unpivoting

### 1. Pivot Columns
```m
// Pivot data
= Table.Pivot(
    Source,
    List.Distinct(Source[Category]),
    "Category",
    "Value"
)
```

### 2. Unpivot Columns
```m
// Unpivot data
= Table.Unpivot(
    Source,
    {"Jan", "Feb", "Mar"},
    "Month",
    "Sales"
)
```

## Grouping and Aggregations

### 1. Group By
```m
// Group and aggregate
= Table.Group(
    Source,
    {"Category"},
    {
        {"TotalSales", each List.Sum([Amount]), type number},
        {"OrderCount", each List.Count([OrderID]), type number}
    }
)
```

### 2. Advanced Grouping
```m
// Multiple grouping levels
= Table.Group(
    Source,
    {"Category", "Region"},
    {
        {"TotalAmount", each List.Sum([Amount]), type number},
        {"AverageAmount", each List.Average([Amount]), type number},
        {"MaxAmount", each List.Max([Amount]), type number}
    }
)
```

## Error Handling

### 1. Replace Errors
```m
// Replace error values
= Table.ReplaceErrorValues(
    Source,
    {{"Amount", 0}, {"Date", null}}
)
```

### 2. Try/Otherwise
```m
// Safe calculations
= Table.AddColumn(
    Source,
    "SafeDivision",
    each try [Amount] / [Quantity] otherwise 0
)
```

## Performance Optimization

### 1. Data Type Optimization
```m
// Use appropriate data types
= Table.TransformColumnTypes(
    Source,
    {
        {"ID", Int64.Type},        // More efficient than text
        {"Amount", Currency.Type}, // Better than number for money
        {"Date", Date.Type}        // More efficient than datetime
    }
)
```

### 2. Remove Unnecessary Columns Early
```m
// Remove columns as early as possible
= Table.RemoveColumns(
    Source,
    {"UnusedColumn1", "UnusedColumn2"}
)
```

### 3. Filter Early
```m
// Apply filters early in the process
= Table.SelectRows(
    Source,
    each [Date] >= #date(2024, 1, 1)
)
```

## Best Practices

### 1. Query Naming
- Use descriptive names
- Include data source in name
- Add version numbers if needed

### 2. Step Organization
- Group related transformations
- Keep steps in logical order
- Remove unnecessary steps

### 3. Error Prevention
- Always check data types
- Validate data quality
- Handle missing values appropriately

### 4. Documentation
- Add comments to complex steps
- Document data sources
- Explain business logic

## Common Patterns

### 1. Data Cleaning Pattern
```m
// Standard data cleaning steps
let
    Source = Excel.Workbook(File.Contents("data.xlsx")),
    RawData = Source{[Name="Sheet1"]}[Data],
    RemovedTopRows = Table.Skip(RawData, 2),
    ChangedTypes = Table.TransformColumnTypes(RemovedTopRows, {
        {"Date", type date},
        {"Amount", type number}
    }),
    RemovedNulls = Table.SelectRows(ChangedTypes, each not List.IsEmpty(Record.FieldValues(_)))
in
    RemovedNulls
```

### 2. Parameterized Query
```m
// Use parameters for flexibility
let
    Source = Excel.Workbook(File.Contents(FilePath)),
    FilteredData = Table.SelectRows(Source, each [Date] >= StartDate and [Date] <= EndDate)
in
    FilteredData
```

## Practice Exercises

### Exercise 1: Basic Transformations
1. Import a CSV file
2. Change data types appropriately
3. Remove unnecessary columns
4. Add a calculated column
5. Filter out null values

### Exercise 2: Data Merging
1. Import two related tables
2. Merge them using appropriate join type
3. Expand the merged columns
4. Clean up the result

### Exercise 3: Advanced Transformations
1. Pivot/unpivot data
2. Group and aggregate
3. Add conditional columns
4. Handle errors appropriately

## Next Steps

Ready to advance? Continue with:
1. [Advanced Power Query](02-advanced-power-query.md)
2. [M Language Deep Dive](03-m-language.md)
3. [Power Query Performance](04-performance-optimization.md)

## Resources

- [Power Query Documentation](https://docs.microsoft.com/en-us/power-query/)
- [M Language Reference](https://docs.microsoft.com/en-us/powerquery-m/)
- [Power Query Community](https://community.powerbi.com/t5/Power-Query/bd-p/power-query)

---

**Master Power Query to transform any data into insights!** 🔧✨ 