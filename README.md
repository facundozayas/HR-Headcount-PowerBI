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

Full project documentation and the ER diagram are provided in the original project file. :contentReference[oaicite:1]{index=1}

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
