# Manufacturing Operations Dashboard 🏭

## Project Overview

This dashboard demonstrates PowerBI solutions for manufacturing, focusing on production efficiency, quality control, and downtime analytics.

## Key Features
- **Production Efficiency**: Output, yield, cycle time, OEE
- **Quality Control**: Defect rates, scrap, rework
- **Downtime Analytics**: Causes, duration, impact
- **Maintenance**: Preventive vs corrective, MTTR, MTBF

## Data Model
### Tables
- **Production**: BatchID, Date, LineID, ProductID, QuantityProduced, CycleTime, Yield
- **Quality**: InspectionID, BatchID, Defects, Scrap, Rework, Inspector
- **Downtime**: EventID, LineID, StartTime, EndTime, Cause, Duration
- **Maintenance**: MaintenanceID, LineID, Type, StartTime, EndTime, Technician
- **Lines**: LineID, Name, Shift, Supervisor

## DAX Examples
```dax
// Overall Equipment Effectiveness (OEE)
OEE = [Availability] * [Performance] * [Quality]

// Defect Rate
Defect Rate = DIVIDE(
    SUM(Quality[Defects]),
    SUM(Production[QuantityProduced]), 0)

// Downtime Percentage
Downtime % = DIVIDE(
    SUM(Downtime[Duration]),
    SUM(Production[CycleTime]) + SUM(Downtime[Duration]), 0)

// MTTR (Mean Time to Repair)
MTTR = AVERAGE(Maintenance[EndTime] - Maintenance[StartTime])
```

## Visualizations
- **KPI Cards**: OEE, defect rate, downtime %
- **Trend Charts**: Production output, downtime events
- **Pareto Charts**: Top downtime causes
- **Heat Maps**: Defects by line/shift
- **Maintenance Analysis**: Preventive vs corrective

## Best Practices
- Integrate with MES/SCADA systems
- Use RLS for plant/line-level access
- Monitor real-time production metrics
- Analyze root causes for downtime

## Sample Data
See `examples/sample-data/manufacturing-production.csv` (create as needed)

## Resources
- [PowerBI for Manufacturing](https://docs.microsoft.com/en-us/power-bi/solutions/industry-manufacturing)

---

**Optimize manufacturing with real-time operations analytics!** 🏭 