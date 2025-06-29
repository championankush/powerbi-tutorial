# Energy & Utilities Analytics Dashboard ⚡

## Project Overview

This dashboard demonstrates PowerBI solutions for energy and utilities, focusing on consumption, production, and outage analytics.

## Key Features
- **Consumption Analytics**: Usage by customer, region, time
- **Production Analytics**: Generation by source, plant, efficiency
- **Outage Analytics**: Frequency, duration, impact
- **Sustainability**: Renewable vs non-renewable, emissions

## Data Model
### Tables
- **Consumption**: RecordID, CustomerID, Date, UsageKWh, Region
- **Production**: PlantID, Date, Source, OutputKWh, Efficiency
- **Outages**: OutageID, Date, Region, Duration, Cause, CustomersAffected
- **Customers**: CustomerID, Name, Type, Region
- **Emissions**: RecordID, Date, Source, CO2Tons

## DAX Examples
```dax
// Average Consumption per Customer
Avg Consumption = AVERAGE(Consumption[UsageKWh])

// Outage Frequency
Outage Frequency = COUNTROWS(Outages)

// Renewable Share
Renewable Share % = DIVIDE(
    SUMX(FILTER(Production, Production[Source] = "Renewable"), Production[OutputKWh]),
    SUM(Production[OutputKWh]), 0)

// Emissions per KWh
Emissions per KWh = DIVIDE(
    SUM(Emissions[CO2Tons]),
    SUM(Production[OutputKWh]), 0)
```

## Visualizations
- **KPI Cards**: Avg consumption, outage frequency, renewable share
- **Trend Charts**: Usage and production over time
- **Bar/Pie Charts**: Source mix, outage causes
- **Heat Maps**: Consumption by region
- **Sustainability Dashboard**: Emissions, renewables

## Best Practices
- Integrate with SCADA/utility systems
- Use RLS for region/customer access
- Monitor real-time outages
- Analyze sustainability metrics

## Sample Data
See `examples/sample-data/energy-consumption.csv` (create as needed)

## Resources
- [PowerBI for Energy](https://docs.microsoft.com/en-us/power-bi/solutions/industry-energy)

---

**Drive energy efficiency and sustainability with actionable analytics!** ⚡ 