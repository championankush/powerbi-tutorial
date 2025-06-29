# Supply Chain & Logistics Analytics Dashboard 🚚

## Project Overview

This dashboard demonstrates PowerBI solutions for supply chain and logistics, focusing on inventory, shipping, and supplier analytics.

## Key Features
- **Inventory Analytics**: Stock levels, turnover, aging
- **Shipping Performance**: On-time delivery, transit time, cost
- **Supplier Analytics**: Performance, lead time, quality
- **Demand Forecasting**: Sales, seasonality, trends

## Data Model
### Tables
- **Inventory**: ItemID, ProductID, Location, StockLevel, LastUpdated, AgingDays
- **Shipments**: ShipmentID, OrderID, Date, Carrier, Status, DeliveryDate, TransitTime, Cost
- **Suppliers**: SupplierID, Name, Rating, LeadTime, QualityScore
- **Orders**: OrderID, ProductID, SupplierID, Date, Quantity, Status
- **Demand**: ProductID, Date, Forecast, Actual

## DAX Examples
```dax
// Inventory Turnover
Inventory Turnover = DIVIDE(
    SUM(Orders[Quantity]),
    AVERAGE(Inventory[StockLevel]), 0)

// On-Time Delivery Rate
On-Time Delivery Rate = DIVIDE(
    COUNTROWS(FILTER(Shipments, Shipments[Status] = "On Time")),
    COUNTROWS(Shipments), 0)

// Supplier Lead Time
Avg Supplier Lead Time = AVERAGE(Suppliers[LeadTime])

// Demand Forecast Accuracy
Forecast Accuracy = 1 - DIVIDE(
    ABS(SUM(Demand[Forecast] - Demand[Actual])),
    SUM(Demand[Actual]), 0)
```

## Visualizations
- **KPI Cards**: Inventory turnover, on-time delivery, forecast accuracy
- **Trend Charts**: Stock levels, shipments over time
- **Bar/Pie Charts**: Supplier performance, order status
- **Heat Maps**: Inventory aging by location
- **Forecasting**: Demand vs actual

## Best Practices
- Integrate with ERP/logistics systems
- Use RLS for supplier/location access
- Monitor real-time shipping performance
- Analyze demand trends

## Sample Data
See `examples/sample-data/supplychain-inventory.csv` (create as needed)

## Resources
- [PowerBI for Supply Chain](https://docs.microsoft.com/en-us/power-bi/solutions/industry-supply-chain)

---

**Optimize supply chain operations with actionable analytics!** 🚚 