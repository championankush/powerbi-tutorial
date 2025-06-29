# Human Resources (HR) Analytics Dashboard 👩‍💼

## Project Overview

This dashboard demonstrates PowerBI solutions for HR, focusing on workforce analytics, attrition, and diversity.

## Key Features
- **Workforce Analytics**: Headcount, hiring, turnover
- **Attrition Analysis**: Voluntary/involuntary, reasons, trends
- **Diversity & Inclusion**: Gender, ethnicity, age, pay equity
- **Employee Engagement**: Survey results, performance reviews

## Data Model
### Tables
- **Employees**: EmployeeID, Name, Gender, Age, Department, HireDate, TerminationDate, Status
- **Attrition**: AttritionID, EmployeeID, Date, Type, Reason
- **Diversity**: EmployeeID, Gender, Ethnicity, PayGrade
- **Engagement**: SurveyID, EmployeeID, Date, Score, Comments
- **Departments**: DepartmentID, Name, Manager

## DAX Examples
```dax
// Turnover Rate
Turnover Rate = DIVIDE(
    COUNTROWS(FILTER(Employees, Employees[Status] = "Terminated")),
    COUNTROWS(Employees), 0)

// Average Tenure
Average Tenure = AVERAGE(Employees[Tenure])

// Gender Diversity
Gender Diversity % = DIVIDE(
    COUNTROWS(FILTER(Employees, Employees[Gender] = "Female")),
    COUNTROWS(Employees), 0)

// Engagement Score
Avg Engagement Score = AVERAGE(Engagement[Score])
```

## Visualizations
- **KPI Cards**: Headcount, turnover, diversity %
- **Trend Charts**: Attrition over time, engagement trends
- **Bar/Pie Charts**: Diversity breakdowns, attrition reasons
- **Department Analysis**: Headcount, engagement by department

## Best Practices
- Use RLS for manager/department access
- Monitor diversity and inclusion metrics
- Analyze exit interview data
- Automate data refresh from HRIS

## Sample Data
See `examples/sample-data/hr-employees.csv` (create as needed)

## Resources
- [PowerBI for HR](https://docs.microsoft.com/en-us/power-bi/solutions/industry-hr)

---

**Drive HR strategy with actionable workforce analytics!** 👩‍💼 