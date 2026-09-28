# 📊 Executive IT Service Desk & Quality Analytics Dashboard (Power BI)

![Power BI](https://img.shields.org/badge/Power_BI-F2C94C?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.org/badge/DAX-00758F?style=for-the-badge&logo=data&logoColor=white)
![Data Modeling](https://img.shields.org/badge/Star_Schema-4169E1?style=for-the-badge) 

An end-to-end, multi-page executive ITSM analytics solution analysing **2,800 ticket records** to give leadership immediate operational visibility into service throughput, SLA delivery, and end-user CSAT trends.

---

## 🎯 Executive Summary & Business ROI

Support desk operations required a consolidated view to evaluate operational bottlenecks and end-user satisfaction. This dashboard separates operational mechanics from quality analytics, providing actionable insights into resolution delays and ticket escalation rates.

* **Primary Business Insight:** Identified a **73.6% dissatisfaction rate** across support operations, directly correlated with extended resolution times averaging **36.6 hours** across all ticket categories.
* **Actionable Outcome:** Outlined clear, data-backed targets for SLA restructuring and priority escalation management.

---

## 📐 Data Architecture & Star Schema

The project utilises a clean **Star Schema** data model, isolating core fact records from dimensional attributes to optimise DAX measure performance and reporting efficiency.

* **`Fact_Tickets`**: Core transactional table containing ticket IDs, response times, resolution durations, priority keys, category keys, and CSAT flags.
* **Dimensions**: `Dim_Date`, `Dim_Category`, `Dim_Priority`, `Dim_CSAT`.

---

## 🛠️ Key DAX Calculations

### 1. Weighted CSAT Index Score (0–100 Scale)
Converts qualitative sentiment (`HIGH`, `MED`, `LOW`) into a standardised executive KPI:

```dax
CSAT Index Score = 
VAR HighCount = CALCULATE(COUNTROWS('Fact_Tickets'), 'Dim_CSAT'[CSAT_Rating] = "HIGH")
VAR MedCount  = CALCULATE(COUNTROWS('Fact_Tickets'), 'Dim_CSAT'[CSAT_Rating] = "MED")
VAR Total     = COUNTROWS('Fact_Tickets')
RETURN
    DIVIDE((HighCount * 100) + (MedCount * 50), Total, 0)

Escalation MoM % = 
VAR CurrentRate = [Escalation %]
VAR PriorRate   = CALCULATE([Escalation %], DATEADD('Dim_Date'[Date], -1, MONTH))
RETURN
    IF(ISBLANK(PriorRate), BLANK(), CurrentRate - PriorRate)

