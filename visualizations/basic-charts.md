# Basic PowerBI Visualizations 📊

## Introduction

PowerBI offers a wide variety of visualizations to help you tell compelling data stories. This guide covers the most commonly used chart types, when to use them, and best practices for effective data visualization.

## Chart Selection Guide

### When to Use Each Chart Type

| Chart Type | Best For | Avoid When |
|------------|----------|------------|
| **Bar Chart** | Comparing categories, ranking items | Too many categories (>10) |
| **Line Chart** | Trends over time, continuous data | Categorical comparisons |
| **Pie Chart** | Parts of a whole (2-5 categories) | Many categories, precise values |
| **Scatter Plot** | Correlation between variables | Categorical data |
| **Table/Matrix** | Detailed data, exact values | Large datasets, trends |
| **Card** | Single KPI values | Multiple values |

## Bar Charts

### Basic Bar Chart
**Use for**: Comparing values across categories

#### Setup:
1. Drag category field to **Axis**
2. Drag measure to **Values**
3. Optional: Add field to **Legend** for grouping

#### Best Practices:
- Sort bars by value (descending or ascending)
- Limit to 10-15 categories maximum
- Use consistent colors
- Add data labels for clarity

#### Example DAX:
```dax
// For sorting
Product Sales Rank = RANKX(
    ALL(Products),
    SUM(Sales[Amount]),
    ,
    DESC
)
```

### Clustered Bar Chart
**Use for**: Comparing multiple measures across categories

#### Setup:
1. Category field to **Axis**
2. Multiple measures to **Values**
3. Optional: Measure to **Legend**

#### Example:
- Product Category on Axis
- Sales Amount and Profit in Values
- Shows both metrics side by side

### Stacked Bar Chart
**Use for**: Showing composition within categories

#### Setup:
1. Category field to **Axis**
2. Measure to **Values**
3. Subcategory to **Legend**

#### Example:
- Region on Axis
- Sales Amount in Values
- Product Category in Legend

## Line Charts

### Basic Line Chart
**Use for**: Showing trends over time

#### Setup:
1. Date field to **Axis**
2. Measure to **Values**
3. Optional: Category to **Legend**

#### Best Practices:
- Use continuous date axis
- Start y-axis at zero when appropriate
- Add markers for key points
- Use smooth lines for trends

#### Example DAX:
```dax
// Rolling average for smooth trend
3-Month Average = AVERAGEX(
    DATESINPERIOD(
        Sales[Date],
        LASTDATE(Sales[Date]),
        -3,
        MONTH
    ),
    SUM(Sales[Amount])
)
```

### Multi-line Chart
**Use for**: Comparing trends across categories

#### Setup:
1. Date field to **Axis**
2. Measure to **Values**
3. Category field to **Legend**

#### Best Practices:
- Limit to 5-7 lines maximum
- Use distinct colors
- Add data labels at endpoints

## Pie Charts

### Basic Pie Chart
**Use for**: Showing parts of a whole

#### Setup:
1. Category field to **Legend**
2. Measure to **Values**

#### Best Practices:
- Use 2-5 categories maximum
- Sort by value (largest first)
- Add percentage labels
- Use contrasting colors

#### Example DAX:
```dax
// Calculate percentage
Sales Percentage = DIVIDE(
    SUM(Sales[Amount]),
    CALCULATE(SUM(Sales[Amount]), ALL(Sales)),
    0
)
```

### Donut Chart
**Use for**: Same as pie chart, but with center space for additional info

#### Setup:
- Same as pie chart
- Adjust inner radius in formatting

## Scatter Plots

### Basic Scatter Plot
**Use for**: Showing correlation between two variables

#### Setup:
1. X-axis measure
2. Y-axis measure
3. Optional: Size field for bubble size
4. Optional: Category to **Legend** for color coding

#### Best Practices:
- Add trend line when appropriate
- Use appropriate axis scales
- Consider log scales for wide ranges

#### Example DAX:
```dax
// Correlation coefficient
Correlation = 
VAR X = Sales[Amount]
VAR Y = Sales[Quantity]
VAR N = COUNTROWS(Sales)
VAR SumX = SUM(X)
VAR SumY = SUM(Y)
VAR SumXY = SUMX(Sales, X * Y)
VAR SumX2 = SUMX(Sales, X * X)
VAR SumY2 = SUMX(Sales, Y * Y)
VAR Correlation = (N * SumXY - SumX * SumY) / 
    SQRT((N * SumX2 - SumX * SumX) * (N * SumY2 - SumY * SumY))
RETURN
    Correlation
```

## Tables and Matrices

### Basic Table
**Use for**: Showing detailed data with exact values

#### Setup:
1. Fields to **Values**
2. Optional: Totals row/column

#### Best Practices:
- Limit columns to essential data
- Use conditional formatting
- Sort by key columns

### Matrix
**Use for**: Cross-tabulation, pivot-like views

#### Setup:
1. Rows field
2. Columns field
3. Values field

#### Example:
- Products on Rows
- Months on Columns
- Sales Amount in Values

## Cards and KPIs

### Card
**Use for**: Single important values

#### Setup:
1. Measure to **Fields**

#### Best Practices:
- Use large, readable fonts
- Add context (title, units)
- Use appropriate number formatting

#### Example DAX:
```dax
// Formatted KPI
Formatted Sales = FORMAT(
    SUM(Sales[Amount]),
    "Currency"
)
```

### KPI Card
**Use for**: Values with targets and status

#### Setup:
1. Measure to **Indicator**
2. Target measure to **Target**
3. Trend measure to **Trend axis**

## Area Charts

### Basic Area Chart
**Use for**: Showing volume over time

#### Setup:
1. Date field to **Axis**
2. Measure to **Values**

#### Best Practices:
- Use for cumulative data
- Consider stacked area for composition
- Start y-axis at zero

### Stacked Area Chart
**Use for**: Showing composition over time

#### Setup:
1. Date field to **Axis**
2. Measure to **Values**
3. Category to **Legend**

## Column Charts

### Basic Column Chart
**Use for**: Same as bar chart, but vertical orientation

#### Best Practices:
- Use for time-based categories
- Limit to 10-15 categories
- Add data labels

### Clustered Column
**Use for**: Comparing multiple measures across categories

### Stacked Column
**Use for**: Showing composition within categories

## Formatting Best Practices

### Color Schemes
```dax
// Consistent color palette
Category Colors = SWITCH(
    Products[Category],
    "Electronics", "#1f77b4",
    "Clothing", "#ff7f0e",
    "Books", "#2ca02c",
    "#d62728"
)
```

### Data Labels
- Show values when space allows
- Use appropriate decimal places
- Include units when relevant

### Titles and Legends
- Use descriptive titles
- Position legends appropriately
- Use clear, concise labels

## Interactive Features

### Drill-through
**Setup**:
1. Create drill-through page
2. Set up drill-through filters
3. Configure drill-through actions

### Cross-filtering
**Setup**:
1. Enable cross-filtering in model view
2. Configure filter direction
3. Test interactions

### Bookmarks
**Setup**:
1. Create bookmark
2. Set view state
3. Add bookmark buttons

## Performance Optimization

### Reduce Visual Complexity
- Limit data points
- Use appropriate aggregations
- Remove unnecessary elements

### Efficient DAX
```dax
// Optimized measure
Efficient Sales = 
VAR CurrentSales = SUM(Sales[Amount])
VAR Result = IF(
    ISBLANK(CurrentSales),
    BLANK(),
    CurrentSales
)
RETURN
    Result
```

## Common Patterns

### 1. KPI Dashboard
```dax
// KPI with status
KPI Status = 
VAR CurrentValue = SUM(Sales[Amount])
VAR TargetValue = [Sales Target]
VAR Status = SWITCH(
    TRUE(),
    CurrentValue >= TargetValue, "On Target",
    CurrentValue >= TargetValue * 0.9, "At Risk",
    "Below Target"
)
RETURN
    Status
```

### 2. Trend Analysis
```dax
// Month-over-month growth
MoM Growth = 
VAR CurrentMonth = SUM(Sales[Amount])
VAR PreviousMonth = CALCULATE(
    SUM(Sales[Amount]),
    PREVIOUSMONTH(Sales[Date])
)
VAR Growth = CurrentMonth - PreviousMonth
VAR GrowthPercent = DIVIDE(Growth, PreviousMonth, 0)
RETURN
    GrowthPercent
```

### 3. Comparative Analysis
```dax
// Year-over-year comparison
YoY Comparison = 
VAR CurrentYear = CALCULATE(
    SUM(Sales[Amount]),
    YEAR(Sales[Date]) = YEAR(TODAY())
)
VAR PreviousYear = CALCULATE(
    SUM(Sales[Amount]),
    YEAR(Sales[Date]) = YEAR(TODAY()) - 1
)
VAR YoYGrowth = DIVIDE(CurrentYear - PreviousYear, PreviousYear, 0)
RETURN
    YoYGrowth
```

## Practice Exercises

### Exercise 1: Sales Dashboard
Create a dashboard with:
1. Sales by product category (bar chart)
2. Monthly sales trend (line chart)
3. Top 5 products (table)
4. Total sales KPI (card)

### Exercise 2: Customer Analysis
Create visualizations for:
1. Customer segments (pie chart)
2. Customer lifetime value vs order count (scatter plot)
3. Customer growth over time (area chart)
4. Regional performance (map)

### Exercise 3: Financial Reporting
Create a financial dashboard with:
1. Revenue vs expenses (clustered column)
2. Profit margin trend (line chart)
3. Budget vs actual (waterfall chart)
4. Key financial ratios (cards)

## Next Steps

Ready to advance? Continue with:
1. [Advanced Visualizations](advanced-charts.md)
2. [Custom Visuals](custom-visuals.md)
3. [Dashboard Design](dashboard-design.md)

## Resources

- [PowerBI Visualization Gallery](https://app.powerbi.com/visuals/)
- [PowerBI Community Visuals](https://community.powerbi.com/t5/Custom-Visuals/bd-p/CustomVisuals)
- [Data Visualization Best Practices](https://www.microsoft.com/en-us/microsoft-365/blog/2016/01/19/data-visualization-best-practices/)

---

**Choose the right chart to tell your data story effectively!** 📈✨ 