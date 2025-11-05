## 🗂️ Folder Structure

```
HR-Headcount-Dashboard/
├── README.md
├── powerbi/HR_Headcount.pbix
├── docs/
│   ├── Project_Report.docx
│   └── Dashboard_Screenshots/
├── scripts/
│   ├── Data_Cleaning.sql
│   └── Calculations_DAX.txt
├── data/sample_data.csv
└── LICENSE
```
# HR-Headcount-PowerBI
HR Headcount &amp; People Analytics Dashboard
**Project type:** Power BI dashboard / Data analysis  
**Authors:** Ximena Galeano, Facundo Zayas  
**Tools:** Power BI Desktop, Power Query, DAX, Excel (data prep)

---

## Short description
This project is a people analytics dashboard designed to provide HR stakeholders with a comprehensive view of workforce metrics such as headcount evolution, employee satisfaction, turnover causes, gender equity, salary distribution, performance by department, and hierarchical structure. The dashboard supports data-driven decisions for recruiters, HR analysts and department managers.

---

## Objectives
- Monitor headcount and workforce growth across years.  
- Identify drivers of attrition (voluntary vs. terminated for cause).  
- Compare performance and satisfaction across departments, managers and salary quartiles.  
- Reveal inequalities (by gender, age group and salary quartile).  
- Provide actionable insights and recommendations for retention and career-path initiatives.

---

## Data sources
The dataset contains HR records (combined from four annual tables: 2016–2019) and includes:
- Personal attributes: `ID`, `FirstName`, `LastName`, `BirthDate`, `Gender`, `Nationality`, `MaritalStatus`.
- Employment attributes: `Position`, `Department`, `ManagerName`, `ManagerID`, `RecruitmentSource`.
- Employment dates and reasons: `HireDate`, `TerminationDate`, `TermReason`, `EmploymentStatus`.
- Performance & compensation: `PerformanceScore`, `EmpSatisfaction`, `Annual Salary`, `Bonus`.
<img width="440" height="265" alt="image" src="https://github.com/user-attachments/assets/35e05cb5-7da0-4a5b-af74-96c83eeb288f" />


Full project documentation and the ER diagram are provided in the original project file, please contact me if you want the file (zayasfacundonicolas@gmail.com)

---

## Data model & transformations
Key preprocessing steps performed in Power Query / data model:
1. **Unify yearly tables**: appended HeadCount tables for 2016–2019 into a single table.  
2. **Clean data**: removed errors, empty rows and duplicates (except `TerminationDate` nulls which mean "still active").  
3. **Dropped `Branch` column** because it contained only nulls.  
4. **Added calendar table** (date dimension) derived from `HireDate` for time analysis.  
5. **Calculated columns** added for analysis (examples below).

### Important calculated columns
- **Year index** — marks the original year source of each record (for growth analysis).  
- **Age**:
  ```dax
  Age = YEAR(TODAY()) - YEAR(HeadCount[BirthDate])
Age Group / Range: < 30, 30–50, > 50

***Salary Quartile (example logic)***
 ```dax
Quartile = 
  IF(HeadCount[Annual Salary] <= PERCENTILE.INC(HeadCount[Annual Salary], 0.25), "Q1",
    IF(HeadCount[Annual Salary] <= PERCENTILE.INC(HeadCount[Annual Salary], 0.5), "Q2",
      IF(HeadCount[Annual Salary] <= PERCENTILE.INC(HeadCount[Annual Salary], 0.75), "Q3", "Q4")))
```
###Key measures (DAX examples)

***Active employees***
```dax
Active_Employees = CALCULATE(DISTINCTCOUNT(HeadCount[ID]), HeadCount[EmploymentStatus] = "Active")
```

***Absenteeism***
```dax
Absenteeism = CALCULATE(DISTINCTCOUNT(HeadCount[ID]), HeadCount[EmploymentStatus] = "Leave of Absence")
```
***Terminated for cause****
```dax
Terminated = CALCULATE(DISTINCTCOUNT(HeadCount[ID]), HeadCount[EmploymentStatus] = "Terminated for Cause")
```
***Voluntary exits***
```dax
Voluntary_Exits = CALCULATE(DISTINCTCOUNT(HeadCount[ID]), HeadCount[EmploymentStatus] = "Voluntarily Terminated
```
***Total turnover***
```dax
Total_Turnover = ([Terminated] + [Voluntary_Exits]) / [HeadCount_Total]
```
***Workforce availability***
```dax
Workforce_Availability = 1 - ([Absenteeism] / [HeadCount_Total])
```
***Average salary / satisfaction***
```dax
Avg_Salary = AVERAGE(HeadCount[Annual Salary])
Avg_Satisfaction = AVERAGE(HeadCount[EmpSatisfaction])
```
_____________________________________________________________________________________
Dashboard pages & visuals

The Power BI report is organized into four pages, each focused on a different analytical perspective:

### Page 1 — Executive Summary

KPIs: average satisfaction, active headcount, total employees, historical exit rate.

Lollipop chart: headcount by year (growth).

Bar chart: average satisfaction by salary quartile.

Area chart: terminated employees by department (historic vs yearly).

Radar chart: department comparison (performance, satisfaction, gender %).

<img width="705" height="398" alt="image" src="https://github.com/user-attachments/assets/3ff0d011-b7cd-446b-b51c-0b1105df823c" />


### Page 2 — Age and Gender Analysis

Filters: Department, Year, Position Level, Nationality.

KPI cards by gender (active employees, turnover %).

Boxplots: salary distribution by gender.

Bar charts: performance and satisfaction comparison by age group and quartile.

<img width="872" height="473" alt="image" src="https://github.com/user-attachments/assets/8ecdcdf3-15fa-4dd0-8174-ba8966b4aa94" />


### Page 3 — Teams & Managers

Filters: Top 5 managers, Level, Year, Department.

KPIs: employees per manager, availability %, avg satisfaction.

Journey / bubble chart: department → level → managers → employees.

Treemap: hierarchical view by department and positions.

<img width="882" height="504" alt="image" src="https://github.com/user-attachments/assets/c0f3ba23-b18b-44e1-aed8-68bdffb9ade9" />


## Key insights & recommendations

Salary vs satisfaction: employees in the lowest salary quartile show significantly lower satisfaction — consider targeted incentives (salary review or non-monetary perks).

Age distribution & career paths: fewer employees >50 and limited representation at higher levels — implement career-path programs to reduce implicit age barriers.

Manager concentration: top managers with the most direct reports are concentrated in production — evaluate span of control and consider redistributing responsibilities to increase horizontal structure.
These recommendations are derived from the exploratory analyses and visualizations included in the report. 

Proyecto coder GALEANO-ZAYAS

