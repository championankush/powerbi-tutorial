# Government & Public Sector Analytics Dashboard 🏛️

## Project Overview

This dashboard demonstrates PowerBI solutions for government and public sector, focusing on public services, budget, and citizen engagement.

## Key Features
- **Public Services**: Service delivery, response times, satisfaction
- **Budget Analytics**: Allocation, spending, variance
- **Citizen Engagement**: Feedback, participation, outreach
- **Compliance**: Transparency, open data, reporting

## Data Model
### Tables
- **Services**: ServiceID, Name, Department, Requests, ResponseTime, Satisfaction
- **Budget**: BudgetID, Department, Year, Allocated, Spent, Variance
- **Citizens**: CitizenID, Name, Region, Feedback, Participation
- **Departments**: DepartmentID, Name, Head, Employees
- **Compliance**: RecordID, Date, Type, Status

## DAX Examples
```dax
// Service Satisfaction Rate
Satisfaction Rate = AVERAGE(Services[Satisfaction])

// Budget Utilization
Budget Utilization % = DIVIDE(
    SUM(Budget[Spent]),
    SUM(Budget[Allocated]), 0)

// Citizen Participation Rate
Participation Rate = DIVIDE(
    COUNTROWS(FILTER(Citizens, Citizens[Participation] = TRUE())),
    COUNTROWS(Citizens), 0)

// Compliance Status
Compliance Status = COUNTROWS(FILTER(Compliance, Compliance[Status] = "Open"))
```

## Visualizations
- **KPI Cards**: Satisfaction rate, budget utilization, participation
- **Trend Charts**: Service requests, budget over time
- **Bar/Pie Charts**: Department spending, compliance status
- **Citizen Engagement**: Feedback, outreach
- **Transparency Dashboard**: Open data, reporting

## Best Practices
- Use RLS for department/region access
- Monitor service delivery metrics
- Analyze citizen feedback
- Automate open data reporting

## Sample Data
See `examples/sample-data/government-services.csv` (create as needed)

## Resources
- [PowerBI for Government](https://docs.microsoft.com/en-us/power-bi/solutions/industry-government)

---

**Enhance public sector transparency and service with actionable analytics!** 🏛️ 