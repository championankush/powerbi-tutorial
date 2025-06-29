# Healthcare Analytics Dashboard 🏥

## Project Overview

This dashboard demonstrates PowerBI solutions for healthcare organizations, focusing on patient outcomes, hospital performance, compliance, and resource utilization.

## Key Features
- **Patient Outcomes**: Readmission rates, mortality, length of stay
- **Hospital Performance**: Bed occupancy, ER wait times, staff utilization
- **Compliance**: HIPAA, quality metrics, incident tracking
- **Resource Utilization**: Equipment, medication, and staff

## Data Model
### Tables
- **Patients**: PatientID, Name, Age, Gender, AdmissionDate, DischargeDate, Diagnosis, Outcome
- **Admissions**: AdmissionID, PatientID, Department, AdmissionDate, DischargeDate, LengthOfStay
- **Staff**: StaffID, Name, Role, Department, Shift, HoursWorked
- **Resources**: ResourceID, Type, Department, Status, Utilization
- **Incidents**: IncidentID, Date, Type, Severity, Department, Resolved

## DAX Examples
```dax
// Readmission Rate
Readmission Rate = DIVIDE(
    CALCULATE(COUNTROWS(Admissions), Admissions[Readmitted] = TRUE()),
    COUNTROWS(Admissions), 0)

// Average Length of Stay
Avg Length of Stay = AVERAGE(Admissions[LengthOfStay])

// Bed Occupancy Rate
Bed Occupancy Rate = DIVIDE(
    SUM(Resources[Utilization]),
    COUNTROWS(Resources), 0)

// Incident Rate
Incident Rate = DIVIDE(
    COUNTROWS(Incidents),
    COUNTROWS(Admissions), 0)
```

## Visualizations
- **KPI Cards**: Readmission rate, avg length of stay, incident rate
- **Trend Charts**: Admissions over time, ER wait times
- **Heat Maps**: Bed occupancy by department
- **Pie/Bar Charts**: Incident types, staff allocation
- **Compliance Dashboard**: HIPAA incidents, resolved vs unresolved

## Best Practices
- Use RLS for patient privacy
- Mask PHI in reports
- Monitor compliance metrics
- Automate data refresh from EHR systems

## Sample Data
See `examples/sample-data/healthcare-patients.csv` (create as needed)

## Resources
- [PowerBI for Healthcare](https://docs.microsoft.com/en-us/power-bi/solutions/industry-healthcare)
- [HIPAA Compliance](https://www.hhs.gov/hipaa/index.html)

---

**Empower healthcare organizations with actionable analytics!** 🏥 