# Sales Analytics Dashboard 📊

## Project Overview

This comprehensive sales analytics dashboard demonstrates real-world PowerBI techniques including advanced DAX, data modeling, and interactive visualizations. The dashboard provides insights into sales performance, customer behavior, and product analytics.

## Dashboard Features

### 📈 Key Metrics
- **Total Sales**: Real-time sales tracking
- **Sales Growth**: Month-over-month and year-over-year comparisons
- **Customer Count**: Active customer tracking
- **Average Order Value**: Customer spending patterns
- **Profit Margin**: Financial performance

### 🎯 Interactive Elements
- **Date Range Slicer**: Dynamic time filtering
- **Product Category Filter**: Category-specific analysis
- **Region Selector**: Geographic performance insights
- **Customer Segment Filter**: Segment-based analysis

### 📊 Visualizations
- **Sales Trend Chart**: Monthly sales performance
- **Product Performance**: Top products by revenue
- **Customer Segmentation**: RFM analysis
- **Geographic Heat Map**: Regional sales distribution
- **Profit Analysis**: Revenue vs cost breakdown

## Data Model

### Tables Structure

#### Sales Table
```sql
Sales (
    SaleID (Primary Key),
    CustomerID (Foreign Key),
    ProductID (Foreign Key),
    SaleDate,
    Quantity,
    UnitPrice,
    UnitCost,
    Region,
    SalesPerson
)
```

#### Products Table
```sql
Products (
    ProductID (Primary Key),
    ProductName,
    Category,
    SubCategory,
    Brand,
    Cost,
    Price
)
```

#### Customers Table
```sql
Customers (
    CustomerID (Primary Key),
    CustomerName,
    Email,
    Region,
    Segment,
    JoinDate
)
```

#### Date Table
```sql
DateTable (
    Date (Primary Key),
    Year,
    Quarter,
    Month,
    MonthName,
    WeekDay,
    IsWeekend
)
```

## DAX Measures

### Core Sales Measures

#### Total Sales
```dax
Total Sales = SUM(Sales[Quantity] * Sales[UnitPrice])
```

#### Total Cost
```dax
Total Cost = SUM(Sales[Quantity] * Sales[UnitCost])
```

#### Gross Profit
```dax
Gross Profit = [Total Sales] - [Total Cost]
```

#### Profit Margin
```dax
Profit Margin % = DIVIDE([Gross Profit], [Total Sales], 0)
```

### Time Intelligence Measures

#### Sales This Year
```dax
Sales YTD = CALCULATE(
    [Total Sales],
    DATESYTD(Sales[SaleDate])
)
```

#### Sales Last Year
```dax
Sales LY = CALCULATE(
    [Total Sales],
    SAMEPERIODLASTYEAR(Sales[SaleDate])
)
```

#### Year-over-Year Growth
```dax
YoY Growth % = DIVIDE(
    [Sales YTD] - [Sales LY],
    [Sales LY],
    0
)
```

#### Month-over-Month Growth
```dax
MoM Growth % = DIVIDE(
    [Total Sales] - CALCULATE(
        [Total Sales],
        PREVIOUSMONTH(Sales[SaleDate])
    ),
    CALCULATE(
        [Total Sales],
        PREVIOUSMONTH(Sales[SaleDate])
    ),
    0
)
```

### Customer Analytics

#### Customer Count
```dax
Customer Count = DISTINCTCOUNT(Sales[CustomerID])
```

#### Average Order Value
```dax
Average Order Value = DIVIDE([Total Sales], DISTINCTCOUNT(Sales[SaleID]), 0)
```

#### Customer Lifetime Value
```dax
Customer LTV = 
VAR CustomerSales = SUM(Sales[Quantity] * Sales[UnitPrice])
VAR CustomerOrders = COUNTROWS(Sales)
VAR AvgOrderValue = DIVIDE(CustomerSales, CustomerOrders, 0)
VAR PurchaseFrequency = CustomerOrders / 12 // Assuming 12 months
VAR LTV = AvgOrderValue * PurchaseFrequency * 12
RETURN
    LTV
```

### Product Analytics

#### Product Performance Score
```dax
Product Score = 
VAR SalesAmount = SUM(Sales[Quantity] * Sales[UnitPrice])
VAR SalesCount = COUNTROWS(Sales)
VAR ProfitMargin = DIVIDE(
    SUM(Sales[Quantity] * (Sales[UnitPrice] - Sales[UnitCost])),
    SalesAmount,
    0
)
VAR Score = (SalesAmount * 0.4) + (SalesCount * 0.3) + (ProfitMargin * 0.3)
RETURN
    Score
```

#### Top Products
```dax
Top 10 Products = CALCULATE(
    [Total Sales],
    TOPN(10, Products, [Total Sales])
)
```

### Advanced Analytics

#### RFM Analysis
```dax
// Recency
Days Since Last Purchase = 
VAR LastPurchase = MAX(Sales[SaleDate])
VAR DaysSince = DATEDIFF(LastPurchase, TODAY(), DAY)
RETURN
    DaysSince

// Frequency
Purchase Frequency = COUNTROWS(Sales)

// Monetary
Total Customer Value = SUM(Sales[Quantity] * Sales[UnitPrice])

// RFM Score
RFM Score = 
VAR RecencyScore = SWITCH(
    TRUE(),
    [Days Since Last Purchase] <= 30, 5,
    [Days Since Last Purchase] <= 60, 4,
    [Days Since Last Purchase] <= 90, 3,
    [Days Since Last Purchase] <= 180, 2,
    1
)
VAR FrequencyScore = SWITCH(
    TRUE(),
    [Purchase Frequency] >= 10, 5,
    [Purchase Frequency] >= 5, 4,
    [Purchase Frequency] >= 3, 3,
    [Purchase Frequency] >= 2, 2,
    1
)
VAR MonetaryScore = SWITCH(
    TRUE(),
    [Total Customer Value] >= 1000, 5,
    [Total Customer Value] >= 500, 4,
    [Total Customer Value] >= 200, 3,
    [Total Customer Value] >= 100, 2,
    1
)
VAR RFMScore = RecencyScore + FrequencyScore + MonetaryScore
RETURN
    RFMScore
```

#### Customer Segmentation
```dax
Customer Segment = 
VAR RFMScore = [RFM Score]
VAR Segment = SWITCH(
    TRUE(),
    RFMScore >= 13, "Champions",
    RFMScore >= 11, "Loyal Customers",
    RFMScore >= 9, "At Risk",
    RFMScore >= 7, "Can't Lose",
    RFMScore >= 5, "Lost",
    "Lost"
)
RETURN
    Segment
```

## Dashboard Setup Instructions

### Step 1: Data Import
1. Open PowerBI Desktop
2. Click **Get Data** → **Excel**
3. Select the sample data file
4. Import all tables: Sales, Products, Customers
5. Create a Date table using the DAX code below

### Step 2: Create Date Table
```dax
// Date Table Creation
DateTable = CALENDAR(
    DATE(2020, 1, 1),
    DATE(2024, 12, 31)
)

// Add date columns
Year = YEAR([Date])
Quarter = "Q" & ROUNDUP(MONTH([Date])/3, 0)
Month = MONTH([Date])
MonthName = FORMAT([Date], "mmmm")
WeekDay = WEEKDAY([Date], 2)
IsWeekend = IF(WeekDay > 5, TRUE, FALSE)
```

### Step 3: Set Up Relationships
1. Go to **Model View**
2. Create relationships:
   - Sales[CustomerID] → Customers[CustomerID]
   - Sales[ProductID] → Products[ProductID]
   - Sales[SaleDate] → DateTable[Date]
3. Set relationship properties:
   - Cross-filter direction: Both
   - Make this relationship active: Yes

### Step 4: Create Measures
1. Go to **Report View**
2. Create all DAX measures listed above
3. Organize measures in display folders

### Step 5: Build Visualizations

#### Page 1: Executive Summary
1. **KPI Cards** (Top row):
   - Total Sales
   - YoY Growth %
   - Customer Count
   - Average Order Value

2. **Sales Trend Chart**:
   - Line chart with Sales YTD and Sales LY
   - Date on axis, measures on values

3. **Product Performance**:
   - Bar chart with top 10 products
   - Product name on axis, Total Sales on values

#### Page 2: Customer Analytics
1. **Customer Segmentation**:
   - Pie chart with Customer Segment
   - Customer Count on values

2. **RFM Analysis**:
   - Scatter plot with RFM Score
   - Customer LTV on size

3. **Geographic Performance**:
   - Map visualization with Region
   - Total Sales on size

#### Page 3: Product Analytics
1. **Category Performance**:
   - Stacked column chart
   - Category on axis, Total Sales on values
   - SubCategory on legend

2. **Profit Analysis**:
   - Waterfall chart
   - Product on axis, Gross Profit on values

3. **Product Score Matrix**:
   - Matrix visualization
   - Product on rows, Product Score on values

### Step 6: Add Interactivity
1. **Slicers**:
   - Date range slicer
   - Product category slicer
   - Region slicer

2. **Filters**:
   - Page-level filters for each page
   - Report-level filters for global filtering

3. **Drill-through**:
   - Create drill-through page for detailed analysis
   - Set up drill-through filters

### Step 7: Formatting
1. **Theme**: Apply consistent color scheme
2. **Fonts**: Use readable fonts and sizes
3. **Layout**: Organize visualizations logically
4. **Titles**: Add descriptive titles and subtitles

## Sample Data

### Sales Data Sample
```csv
SaleID,CustomerID,ProductID,SaleDate,Quantity,UnitPrice,UnitCost,Region,SalesPerson
1,101,201,2024-01-15,2,150.00,100.00,North,John Smith
2,102,202,2024-01-16,1,200.00,120.00,South,Jane Doe
3,103,203,2024-01-17,3,75.00,50.00,East,Mike Johnson
```

### Products Data Sample
```csv
ProductID,ProductName,Category,SubCategory,Brand,Cost,Price
201,Laptop Pro,Electronics,Computers,TechCorp,100.00,150.00
202,Smartphone X,Electronics,Phones,MobileTech,120.00,200.00
203,Wireless Headphones,Electronics,Audio,SoundMax,50.00,75.00
```

### Customers Data Sample
```csv
CustomerID,CustomerName,Email,Region,Segment,JoinDate
101,Alice Johnson,alice@email.com,North,Premium,2023-01-15
102,Bob Smith,bob@email.com,South,Regular,2023-03-20
103,Carol Davis,carol@email.com,East,Budget,2023-06-10
```

## Performance Optimization

### Data Model Optimization
1. **Data Types**: Use appropriate data types
2. **Relationships**: Optimize relationship cardinality
3. **Calculated Columns**: Minimize calculated columns
4. **Measures**: Use measures for aggregations

### Query Optimization
1. **Filters**: Apply filters early in the process
2. **Aggregations**: Use appropriate aggregation levels
3. **Caching**: Leverage PowerBI's caching mechanisms

## Troubleshooting

### Common Issues
1. **Relationship Errors**: Check data types and cardinality
2. **Performance Issues**: Optimize DAX measures and data model
3. **Visualization Errors**: Verify field mappings and data types

### Best Practices
1. **Naming Conventions**: Use consistent naming
2. **Documentation**: Document complex measures
3. **Testing**: Test with various data scenarios
4. **Version Control**: Save multiple versions during development

## Next Steps

After completing this dashboard:
1. **Customize**: Adapt to your specific business needs
2. **Extend**: Add more advanced analytics
3. **Deploy**: Publish to PowerBI Service
4. **Share**: Distribute to stakeholders

## Resources

- [PowerBI Documentation](https://docs.microsoft.com/en-us/power-bi/)
- [DAX Reference](https://docs.microsoft.com/en-us/dax/)
- [PowerBI Community](https://community.powerbi.com/)

---

**Build this dashboard to master real-world PowerBI development!** 🚀 