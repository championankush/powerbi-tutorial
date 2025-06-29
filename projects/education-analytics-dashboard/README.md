# Education Analytics Dashboard 🎓

## Project Overview

This dashboard demonstrates PowerBI solutions for education, focusing on student performance, attendance, and outcomes.

## Key Features
- **Student Performance**: Grades, GPA, subject analysis
- **Attendance**: Absenteeism, tardiness, trends
- **Outcomes**: Graduation rates, college placement
- **Demographics**: Gender, ethnicity, socioeconomic status

## Data Model
### Tables
- **Students**: StudentID, Name, Gender, Ethnicity, GradeLevel
- **Grades**: GradeID, StudentID, Subject, Score, Term, Teacher
- **Attendance**: AttendanceID, StudentID, Date, Status (Present/Absent/Tardy)
- **Outcomes**: StudentID, GraduationStatus, CollegePlacement
- **Teachers**: TeacherID, Name, Subject, YearsExperience

## DAX Examples
```dax
// Average GPA
Average GPA = AVERAGE(Grades[Score])

// Attendance Rate
Attendance Rate = DIVIDE(
    COUNTROWS(FILTER(Attendance, Attendance[Status] = "Present")),
    COUNTROWS(Attendance), 0)

// Graduation Rate
Graduation Rate = DIVIDE(
    COUNTROWS(FILTER(Outcomes, Outcomes[GraduationStatus] = "Graduated")),
    COUNTROWS(Outcomes), 0)

// Subject Performance
Math Performance = AVERAGE(FILTER(Grades, Grades[Subject] = "Math")[Score])
```

## Visualizations
- **KPI Cards**: Average GPA, attendance rate, graduation rate
- **Trend Charts**: Attendance over time, grade trends
- **Bar Charts**: Performance by subject, teacher
- **Demographics**: Student breakdowns
- **Outcome Analysis**: College placement rates

## Best Practices
- Use RLS for student/teacher access
- Monitor at-risk students
- Analyze performance gaps
- Automate data refresh from SIS/LMS

## Sample Data
See `examples/sample-data/education-students.csv` (create as needed)

## Resources
- [PowerBI for Education](https://docs.microsoft.com/en-us/power-bi/solutions/industry-education)

---

**Empower educators with actionable student analytics!** 🎓 