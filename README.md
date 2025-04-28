# patient-risk-monitoring

This project focuses on building a Power BI decision support system for monitoring key healthcare quality KPIs across multiple divisions.  
It covers the full lifecycle from requirements gathering, Data Mart construction, ETL data cleaning, automated data refresh, dashboard development, and Row-Level Security (RLS) implementation.

---

## 📌 Project Overview

To support healthcare quality improvement initiatives, we developed a dynamic KPI monitoring dashboard, replacing previous weekly and monthly static reports.  
This new system enhances real-time visibility and operational efficiency for frontline healthcare teams and management.

---

## 🧰 Features

- **Requirements Gathering and Indicator Refinement**  
  Collaborated with frontline staff to refine existing KPI definitions and propose new indicators.

- **Data Mart Construction**  
  Integrated multi-source data using ETL tools and built a centralized Data Mart tailored for dashboard consumption.

- **ETL Data Pipeline Automation**  
  Designed ETL workflows for daily automated updates, eliminating the need for manual reporting and emailing routines.

- **Dynamic Dashboard Development**  
  Developed Power BI dashboards featuring:
  - 7 key performance indicators (KPIs)
  - Trend analysis and division comparisons
  - Interactive filters (year, quarter, unit, department)

- **Row-Level Security (RLS)**  
  Implemented RLS for controlled data access based on user roles.

- **Efficiency Gains**  
  - Saved approximately 15 hours of manual work per month
  - Improved data timeliness, security, and flexibility

---

## 🧱 Tech Stack

| Tool          | Purpose                          |
|---------------|----------------------------------|
| SQL Server    | Data integration and extraction  |
| ETL Tools     | Data cleaning and transformation |
| Power BI      | Data modeling, visualization, and RLS |
| DAX           | KPI calculations and filtering   |

---

## 📈 Workflow Overview

1. **Requirements Analysis** → 2. **Data Mart Building** → 3. **ETL Process Design** → 4. **Power BI Development** → 5. **Deployment and Maintenance**

Example ETL Flow Diagram:

📄 `/doc/etl_process_flow.png` (uploaded separately)

Example Dashboard Screenshot:

📄 `/images/kpi_dashboard_sample.png` (uploaded separately)

---

## 📊 DAX Sample Calculations

The following DAX measures calculate the achievement rate of a key healthcare quality KPI.  
Although both measures are based on the same core formula, different filter contexts are applied to enable simultaneous comparison between the overall performance and branch-level performance.

### KPI Achievement Rate - Overall

```DAX
KPI_AchievementRate_Overall = 
CALCULATE(
    DIVIDE(
        SUM('column_name'[numerator]),
        SUM('fact_assessment'[denominator])
    ),
    FILTER(
        ALLEXCEPT(
            'fact_assessment',
            'fact_assessment'[Interval],
            'fact_assessment'[Date_Frequency],
            'fact_assessment'[Year],
            'fact_assessment'[Department]
        ),
        'fact_assessment'[Hospital_Zone_Order] = "0.All"
    )
)
```
Explanation:
KPI_AchievementRate_All calculates the overall achievement rate across all branches combined.

 
### KPI Achievement Rate - Branch

```DAX
KPI_AchievementRate_Branch = 
CALCULATE(
    DIVIDE(
        SUM('fact_assessment'[PEOL_NUMBER]),
        SUM('fact_assessment'[ASSE_NUMBER])
    ),
    FILTER(
        'fact_assessment',
        'fact_assessment'[Hospital_Zone_Order] <> "0.All"
    )
)
```
Explanation:
KPI_AchievementRate_Branch calculates the achievement rate for each individual branch separately.
