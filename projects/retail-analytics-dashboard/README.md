# Retail Sales & Inventory Analytics Dashboard 🛒

## Project Overview

This dashboard demonstrates PowerBI solutions for retail, focusing on sales performance, inventory management, and customer analytics.

## Key Features
- **Sales Performance**: Revenue, profit, sales by product/category/store
- **Inventory Management**: Stock levels, turnover, out-of-stock alerts
- **Customer Analytics**: Segmentation, loyalty, basket analysis
- **Promotions**: Campaign effectiveness, discount impact

## Data Model
### Tables
- **Sales**: SaleID, Date, StoreID, ProductID, Quantity, UnitPrice, Discount, CustomerID
- **Products**: ProductID, Name, Category, Brand, Cost, Price, StockLevel
- **Stores**: StoreID, Name, Location, Manager
- **Customers**: CustomerID, Name, Segment, LoyaltyPoints
- **Promotions**: PromoID, ProductID, StartDate, EndDate, Discount

## DAX Examples
```dax
// Gross Profit
Gross Profit = SUMX(Sales, Sales[Quantity] * (Sales[UnitPrice] - Products[Cost]))

// Inventory Turnover
Inventory Turnover = DIVIDE(
    SUM(Sales[Quantity]),
    AVERAGE(Products[StockLevel]), 0)

// Out-of-Stock Rate
Out of Stock Rate = DIVIDE(
    COUNTROWS(FILTER(Products, Products[StockLevel] = 0)),
    COUNTROWS(Products), 0)

// Customer Loyalty Segment
Loyalty Segment = SWITCH(
    TRUE(),
    Customers[LoyaltyPoints] > 1000, "Platinum",
    Customers[LoyaltyPoints] > 500, "Gold",
    Customers[LoyaltyPoints] > 100, "Silver",
    "Bronze")
```

## Visualizations
- **KPI Cards**: Revenue, profit, inventory turnover
- **Trend Charts**: Sales over time, stock levels
- **Bar/Pie Charts**: Sales by category, out-of-stock products
- **Customer Segmentation**: Loyalty, basket size
- **Promotion Analysis**: Campaign ROI

## Best Practices
- Automate data refresh from POS systems
- Use RLS for store-level access
- Monitor inventory in real-time
- Analyze promotion effectiveness

## Sample Data
See `examples/sample-data/retail-sales.csv` (create as needed)

## Resources
- [PowerBI for Retail](https://docs.microsoft.com/en-us/power-bi/solutions/industry-retail)

---

**Drive retail success with actionable sales and inventory analytics!** 🛒 