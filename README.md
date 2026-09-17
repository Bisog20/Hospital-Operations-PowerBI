# Hospital Operations & Patient Care Analytics — Power BI

An end-to-end Power BI dashboard analyzing hospital admissions, patient demographics, and cost drivers, built from a raw Kaggle healthcare dataset through to a polished, navigable 3-page report.

## Dataset

`healthcare_dataset.csv` — 55,500 raw patient records (Name, Age, Gender, Blood Type, Medical Condition, Admission/Discharge Dates, Doctor, Hospital, Insurance Provider, Billing Amount, Room Number, Admission Type, Medication, Test Results).

## Data Cleaning (Power Query)

- Proper-cased inconsistent name casing (`Bobby JacksOn` → `Bobby Jackson`)
- Converted `Date of Admission` and `Discharge Date` from text to proper Date types
- Removed 534 exact duplicate rows
- Converted 108 negative `Billing Amount` values to absolute values (treated as data entry errors)
- Derived a `Length of Stay` column (Discharge Date − Date of Admission)
- Trimmed and cleaned all text columns

## Data Model

Star schema: a dedicated `Date` dimension table (built with `CALENDAR()`, marked as an official Date table) joined to the `Patients` fact table — an active relationship on `Date of Admission`, and an inactive relationship on `Discharge Date` (accessed via `USERELATIONSHIP()` for discharge-specific measures).

## DAX Measures

Core KPIs (Total Admissions, Avg Length of Stay, Total Billing, Avg Billing per Patient) plus time-intelligence measures (Admissions MoM %, Billing YTD) and a cross-relationship measure (Total Discharges, using `USERELATIONSHIP`).

## Report Pages

1. **Executive Overview** — KPI cards, admissions trend, admissions by medical condition
2. **Demographics** — age distribution, gender split, medical condition by gender, blood type breakdown
3. **Admission & Capacity** — room utilization, avg length of stay by condition, admission type breakdown, yearly admissions trend by type

Each page includes slicers (Admission Type, Medical Condition, Date) and click-through page navigation buttons.

## Screenshots

**Executive Overview**
![Executive Overview](screenshots/executive_overview.png)

**Demographics**
![Demographics](screenshots/demographics.png)

**Admission & Capacity**
![Admission & Capacity](screenshots/admission_capacity.png)

## Known Issue

The Executive Overview KPI card currently shows **40K Total Admissions**, but the fully cleaned dataset totals **54,966** rows (55,500 − 534 duplicates). This points to an unresolved filter, slicer, or relationship somewhere in the report unintentionally excluding roughly 15,000 rows from that visual specifically — the Room Utilization table on the Admission & Capacity page shows a closer total of 40,235, suggesting the discrepancy is isolated to certain visuals rather than the underlying data model. Flagged here rather than hidden; tracing and fixing this filter is the next open item on this project.

## Files

- `Healthcare.pbix` — the full Power BI report
- `healthcare_dataset.csv` — source dataset
- `screenshots/` — page exports (Executive Overview, Demographics, Admission & Capacity)

## Tools

Power BI Desktop — Power Query (M), DAX, star schema data modeling.
