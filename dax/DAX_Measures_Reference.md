# DAX Measures Reference Guide

**Project**: Construction Project Cost & Timeline Dashboard  
**Academic Level**: 3rd-Year B.E. Computer Science Engineering, Sathyabama Institute of Science & Technology  
**File**: `_Measures` Table in Power BI Desktop  

---

## Overview

In Power BI, DAX (Data Analysis Expressions) measures are dynamic formulas evaluated on the fly based on the active filter context (slicers, visual rows, cross-filtering). Unlike calculated columns which consume RAM and are evaluated during data load, DAX measures are computationally efficient and adjust dynamically.

All measures below are organized in a dedicated table named `_Measures`.

---

## 1. Financial & Cost Measures

### Measure 1: Total Budget
```dax
Total Budget = SUM(Projects[Budget])
```
- **Syntax**: `SUM(Projects[Budget])` aggregates the sanctioned capital expenditure budget for all projects currently in context.
- **Why Needed**: Serves as the high-level financial ceiling against which commitments and expenditures are audited.
- **Format**: Currency -> `INR (₹)` or `English (India)`, 2 decimal places / Whole number.
- **Dataset Value (All Projects)**: ₹7,15,00,00,000 (₹715.00 Cr).

---

### Measure 2: Planned Cost
```dax
Planned Cost = SUM(Project_Cost[Planned Cost])
```
- **Syntax**: Sums the cumulative scheduled/planned cost up to the current date or selected period across projects.
- **Why Needed**: Establishes the Planned Value (PV) baseline according to the master construction work breakdown schedule.
- **Format**: Currency -> `INR (₹)`, 2 decimal places.
- **Dataset Value (To Date)**: ₹5,37,50,05,000 (₹537.50 Cr).

---

### Measure 3: Actual Cost
```dax
Actual Cost = SUM(Project_Cost[Actual Cost])
```
- **Syntax**: Aggregates the real financial expenditure (Actual Cost / AC) incurred on materials, labor, sub-contractors, and equipment.
- **Why Needed**: Measures actual capital burn rate. Essential for variance analysis against the planned cost.
- **Format**: Currency -> `INR (₹)`, 2 decimal places.
- **Dataset Value (To Date)**: ₹5,76,73,38,000 (₹576.73 Cr).

---

### Measure 4: Cost Variance
```dax
Cost Variance = [Actual Cost] - [Planned Cost]
```
- **Syntax**: Subtracts planned baseline cost from the actual cost incurred.
- **Why Needed**: Fundamental metric for financial health.
- **Interpretation**:
  - `Cost Variance > 0`: **Unfavorable / Over Budget** (Expenditure exceeds planned baseline).
  - `Cost Variance < 0`: **Favorable / Under Budget** (Savings achieved or lower expenditure).
  - `Cost Variance = 0`: **On Budget**.
- **Format**: Currency -> `INR (₹)`, 2 decimal places.
- **Dataset Value**: +₹39,23,33,000 (+₹39.23 Cr overrun across the portfolio).

---

### Measure 5: Budget Deviation %
```dax
Budget Deviation % = 
DIVIDE([Cost Variance], [Planned Cost], 0)
```
- **Syntax**: Uses `DIVIDE()` to safely compute the percentage deviation from the planned cost while avoiding `#DIV/0!` errors when planned cost is 0.
- **Why Needed**: Normalizes cost overruns across small and mega projects. A ₹5 Cr overrun on a ₹30 Cr project (16.7%) is far more severe than on a ₹500 Cr project (1%).
- **Interpretation**: Positive indicates percentage cost overrun; negative indicates percentage cost savings.
- **Format**: Percentage (`%`), 2 decimal places.
- **Dataset Value**: **+7.30%**.

---

## 2. Timeline, Schedule & Progress Measures

### Measure 6: Project Completion %
```dax
Project Completion % = 
MAX(Project_Progress[Actual Progress %])
```
- **Syntax**: Extracts the latest recorded cumulative actual progress percentage for the project in context.
- **Why Needed**: Directly communicates the ground physical progress achieved on site (civil, structural, architectural, MEP).
- **Format**: Percentage (`%`), 2 decimal places.
- **Dataset Value**: Ranges from 70.0% to 100.0% per project.

---

### Measure 7: Planned Progress %
```dax
Planned Progress % = 
MAX(Project_Progress[Planned Progress %])
```
- **Syntax**: Retrieves the expected cumulative milestone completion percentage as planned by the project engineering office up to the current date.
- **Why Needed**: Benchmark to assess whether work on site matches the engineering schedule.
- **Format**: Percentage (`%`), 2 decimal places.
- **Dataset Value**: Ranges from 74.0% to 100.0% per project.

---

### Measure 8: Progress Gap / Delay %
```dax
Progress Gap = 
[Planned Progress %] - [Project Completion %]
```
- **Syntax**: Subtracts actual progress achieved from planned scheduled progress.
- **Why Needed**: Serves as the quantitative delay indicator.
- **Interpretation**:
  - `Progress Gap > 0`: **Behind Schedule / Delayed** (Physical site progress lags the planned schedule).
  - `Progress Gap = 0`: **Exactly On Schedule** or completed.
  - `Progress Gap < 0`: **Ahead of Schedule**.
- **Format**: Percentage (`%`), 2 decimal places.
- **Dataset Value**: Portfolio average gap is **+6.70%**. Highest in PRJ005 (Velachery Mall: +20.0%) and PRJ001 (Chennai Metro: +16.0%).

---

### Measure 9: Average Project Completion %
```dax
Average Project Completion % = 
AVERAGEX(Projects, [Project Completion %])
```
- **Syntax**: Iterates over all projects in the `Projects` table using `AVERAGEX` and averages each individual project's latest completion percentage.
- **Why Needed**: Summarizes portfolio-wide physical progress on top-level executive KPI cards without suffering from row-count weighting distortions in monthly progress logs.
- **Format**: Percentage (`%`), 2 decimal places.
- **Dataset Value**: **83.70%**.

---

## 3. Project Count & Status Measures

### Measure 10: Total Projects
```dax
Total Projects = 
DISTINCTCOUNT(Projects[Project ID])
```
- **Syntax**: Counts unique project IDs in the active filter context.
- **Why Needed**: Baseline volume counter for projects under management.
- **Format**: Whole number (`0`).
- **Dataset Value**: **10**.

---

### Measure 11: Completed Projects
```dax
Completed Projects = 
CALCULATE(
    DISTINCTCOUNT(Projects[Project ID]),
    Projects[Status] = "Completed"
)
```
- **Syntax**: Uses `CALCULATE` to modify the filter context, counting projects where status equals "Completed".
- **Why Needed**: Tracks projects handed over to clients.
- **Format**: Whole number (`0`).
- **Dataset Value**: **2** (PRJ003 Marina Coastal Promenade, PRJ009 ECR Luxury Enclave).

---

### Measure 12: Delayed Projects
```dax
Delayed Projects = 
CALCULATE(
    DISTINCTCOUNT(Projects[Project ID]),
    Projects[Status] = "Delayed"
)
```
- **Syntax**: Counts projects flagged as "Delayed" due to schedule slippage or material hold-ups.
- **Why Needed**: Key operational risk KPI requiring high-level intervention.
- **Format**: Whole number (`0`).
- **Dataset Value**: **3** (PRJ001 Metro Line 4, PRJ005 Velachery Mall, PRJ007 Sriperumbudur EV Plant).

---

### Measure 13: Projects Over Budget
```dax
Projects Over Budget = 
CALCULATE(
    DISTINCTCOUNT(Projects[Project ID]),
    FILTER(Projects, [Cost Variance] > 0)
)
```
- **Syntax**: Filters the `Projects` table where the evaluated `[Cost Variance]` measure is strictly positive.
- **Why Needed**: Identifies cost-control vulnerability across project portfolios.
- **Format**: Whole number (`0`).
- **Dataset Value**: **8 of 10** projects currently exhibit cost overruns to varying degrees.

---

## 4. Milestone Governance Measures

### Measure 14: Total Milestones
```dax
Total Milestones = 
COUNTROWS(Milestones)
```
- **Syntax**: Evaluates the total number of contractual or engineering stage-gates recorded in the `Milestones` table.
- **Why Needed**: Foundation for tracking milestone execution velocity.
- **Format**: Whole number (`0`).
- **Dataset Value**: **50** (5 major milestones across 10 projects).

---

### Measure 15: Completed Milestones
```dax
Completed Milestones = 
CALCULATE(
    COUNTROWS(Milestones),
    Milestones[Status] = "Completed"
)
```
- **Syntax**: Counts milestones where status is "Completed".
- **Why Needed**: Validates delivery milestones signed off by engineering quality inspectors.
- **Format**: Whole number (`0`).
- **Dataset Value**: **34**.

---

### Measure 16: Pending Milestones
```dax
Pending Milestones = 
CALCULATE(
    COUNTROWS(Milestones),
    Milestones[Status] = "In Progress"
)
```
- **Syntax**: Counts milestones that are actively underway on site.
- **Why Needed**: Shows upcoming operational workloads for engineering teams.
- **Format**: Whole number (`0`).
- **Dataset Value**: **12**.

---

### Measure 17: Delayed Milestones
```dax
Delayed Milestones = 
CALCULATE(
    COUNTROWS(Milestones),
    Milestones[Status] = "Delayed"
)
```
- **Syntax**: Counts milestones that have breached their scheduled target completion date.
- **Why Needed**: Red-flag indicator for immediate site superintendent attention.
- **Format**: Whole number (`0`).
- **Dataset Value**: **4** (MLS004 Viaduct Erection, MLS024 Multiplex Shell, MLS034 Conveyor Cells, MLS035 High-Tension Substation).

---

### Measure 18: Milestone Completion %
```dax
Milestone Completion % = 
DIVIDE([Completed Milestones], [Total Milestones], 0)
```
- **Syntax**: Ratio of completed stage-gates to total stage-gates.
- **Why Needed**: Holistic project governance metric reflecting milestone delivery health.
- **Format**: Percentage (`%`), 2 decimal places.
- **Dataset Value**: **68.00%** (34 of 50 milestones achieved).

---

## Quick Reference Summary Table

| # | Measure Name | DAX Expression | Default Format | Portfolio Benchmark |
|---|---|---|---|---|
| 1 | **Total Budget** | `SUM(Projects[Budget])` | Currency (₹) | ₹7,15,00,00,000 |
| 2 | **Planned Cost** | `SUM(Project_Cost[Planned Cost])` | Currency (₹) | ₹5,37,50,05,000 |
| 3 | **Actual Cost** | `SUM(Project_Cost[Actual Cost])` | Currency (₹) | ₹5,76,73,38,000 |
| 4 | **Cost Variance** | `[Actual Cost] - [Planned Cost]` | Currency (₹) | +₹39,23,33,000 |
| 5 | **Budget Deviation %** | `DIVIDE([Cost Variance], [Planned Cost], 0)` | Percentage (%) | +7.30% |
| 6 | **Project Completion %** | `MAX(Project_Progress[Actual Progress %])` | Percentage (%) | Project Specific |
| 7 | **Planned Progress %** | `MAX(Project_Progress[Planned Progress %])` | Percentage (%) | Project Specific |
| 8 | **Progress Gap** | `[Planned Progress %] - [Project Completion %]` | Percentage (%) | +6.70% (Avg) |
| 9 | **Average Project Completion %** | `AVERAGEX(Projects, [Project Completion %])` | Percentage (%) | 83.70% |
| 10 | **Total Projects** | `DISTINCTCOUNT(Projects[Project ID])` | Whole Number | 10 |
| 11 | **Completed Projects** | `CALCULATE(DISTINCTCOUNT(Projects[Project ID]), Projects[Status] = "Completed")` | Whole Number | 2 |
| 12 | **Delayed Projects** | `CALCULATE(DISTINCTCOUNT(Projects[Project ID]), Projects[Status] = "Delayed")` | Whole Number | 3 |
| 13 | **Projects Over Budget** | `CALCULATE(DISTINCTCOUNT(Projects[Project ID]), FILTER(Projects, [Cost Variance] > 0))` | Whole Number | 8 |
| 14 | **Total Milestones** | `COUNTROWS(Milestones)` | Whole Number | 50 |
| 15 | **Completed Milestones** | `CALCULATE(COUNTROWS(Milestones), Milestones[Status] = "Completed")` | Whole Number | 34 |
| 16 | **Pending Milestones** | `CALCULATE(COUNTROWS(Milestones), Milestones[Status] = "In Progress")` | Whole Number | 12 |
| 17 | **Delayed Milestones** | `CALCULATE(COUNTROWS(Milestones), Milestones[Status] = "Delayed")` | Whole Number | 4 |
| 18 | **Milestone Completion %** | `DIVIDE([Completed Milestones], [Total Milestones], 0)` | Percentage (%) | 68.00% |
