# Presentation Slides Structure (12 Slides)

**Project Title**: Construction Project Cost & Timeline Dashboard  
**Candidate**: 3rd-Year B.E. Computer Science Engineering, Sathyabama Institute of Science & Technology  
**Evaluation**: PT-1 Data Analytics Review  

---

## Slide 1: Title Slide
- **Title**: Construction Project Cost & Timeline Dashboard
- **Subtitle**: Multi-Table Relational Data Analytics & BI Governance in Civil Engineering
- **Student Name & Register Number**: [Your Name] | [Your Register Number]
- **Department**: Department of Computer Science and Engineering
- **Institution**: Sathyabama Institute of Science and Technology, Chennai
- **Tools Used**: Microsoft Power BI Desktop, Power Query, DAX, Excel

---

## Slide 2: Introduction & Project Context
- **Industry Challenge**: Large construction projects frequently suffer from severe cost escalations and schedule slippages.
- **Traditional Limitation**: Disconnected spreadsheets, delayed paper reports, and lack of real-time visibility for project managers.
- **Solution**: A centralized, interactive Business Intelligence dashboard developed in Microsoft Power BI.
- **Geographic Focus**: 10 high-value construction and infrastructure projects across Chennai & Tamil Nadu.

---

## Slide 3: Problem Statement & Objectives
- **Problem Statement**: Inability to concurrently monitor financial burn rate against on-site physical progress leads to unforeseen budget blowouts.
- **Core Objectives**:
  1. Build a clean 4-table relational data model (Projects, Cost, Progress, Milestones).
  2. Implement Star Schema architecture with 1-to-many relationships.
  3. Formulate 18+ DAX measures for Cost Variance, Delay %, and Milestone ratios.
  4. Design an executive dashboard with interactive cross-filtering and KPIs.
  5. Deliver actionable project management recommendations.

---

## Slide 4: Dataset Overview & Schema Architecture
- **Data Source**: `Construction_Project_Data.xlsx` (4 logical tables).
  - `Projects` (10 records): Primary master dimension table (Budget, Manager, Type, Status).
  - `Project_Cost` (153 records): Monthly Planned vs Actual expenditure in INR.
  - `Project_Progress` (153 records): Monthly Planned vs Actual cumulative completion %.
  - `Milestones` (50 records): Stage-gate milestone tracking (Planned vs Actual Dates).
- **Data Model**: 1-to-Many Star Schema connected via `Project ID`. Single cross-filter direction ensures high query efficiency.

---

## Slide 5: Power Query ETL & Data Cleaning
- **ETL Transformations**:
  - Ingested multi-sheet workbook and promoted column headers.
  - Assigned strict data types (`Int64` for Cost/Budget, `Number` for Percentages, `Date` for milestones).
  - Handled legitimate `null` values in pending milestone completion dates.
  - Verified primary key uniqueness (`Table.Distinct` on `Project ID`).
  - Formed a dedicated `_Measures` table for organized DAX governance.

---

## Slide 6: Key DAX Calculations & KPI Engineering
- **Cost Variance**:
  `Cost Variance = [Actual Cost] - [Planned Cost]`
- **Budget Deviation %**:
  `Budget Deviation % = DIVIDE([Cost Variance], [Planned Cost], 0)`
- **Progress Gap (Delay %)**:
  `Progress Gap = [Planned Progress %] - [Project Completion %]`
- **Milestone Completion Rate**:
  `Milestone Completion % = DIVIDE([Completed Milestones], [Total Milestones], 0)`
- *Safe division prevents `#DIV/0!` errors; dynamic row-level evaluation.*

---

## Slide 7: Dashboard Architecture & Visual Design
- **Canvas Layout**: Professional 16:9 grid (1280 x 720) with high readability.
- **Visual Components**:
  - **Top Banner**: 5 Primary KPI Cards (Budget, Spend, Variance, Progress %, Dev %).
  - **Left Navigation**: Interactive Slicers (Project, Type, Manager, Status).
  - **Center Charts**: Monthly Planned vs Actual Spend (Bar) & Progress S-Curve (Line).
  - **Right Visuals**: Budget Deviation % ranking & Milestone Status Donut.
  - **Bottom Table**: Comprehensive Project Audit Matrix with conditional formatting.

---

## Slide 8: Key Analytical Findings (The Numbers)
- **Total Portfolio Budget**: INR 715.00 Crores across 10 active projects.
- **Planned vs Actual Spend**: INR 537.50 Cr Planned vs INR 576.73 Cr Actual.
- **Portfolio Cost Variance**: **+INR 39.23 Crores (+7.30% budget overrun)**.
- **Average Project Completion**: **83.70%** achieved on site.
- **Average Schedule Gap**: **+6.70%** behind planned baseline.
- **Milestones**: 34 Completed (68%), 12 In Progress, 4 Delayed.

---

## Slide 9: Critical Project Deep-Dive
- **Highest Cost Overruns**:
  1. *Chennai Metro Line 4* (`PRJ001`): +INR 14.95 Cr overrun (+16.61%), 16% delay.
  2. *Sriperumbudur EV Plant* (`PRJ007`): +INR 13.97 Cr overrun (+14.94%), 13% delay.
  3. *Velachery Commercial Mall* (`PRJ005`): +INR 10.61 Cr overrun (+16.00%), 20% delay.
- **Best Cost & Schedule Performers**:
  - *Marina Coastal Promenade* (`PRJ003`): -INR 2.19 Cr (-6.25% under budget), 100% complete.
  - *ECR Luxury Enclave* (`PRJ009`): -INR 2.11 Cr (-5.27% under budget), 100% complete.

---

## Slide 10: Actionable Project Management Recommendations
- **Immediate Scope Freeze**: Stop design iterations on Velachery Mall to halt fitout cost expansion.
- **Equipment Expediting**: Fast-track customs clearance for imported cleanroom conveyor machinery at Sriperumbudur EV Plant.
- **Crash Scheduling**: Deploy extra pier casting shuttering on Chennai Metro Line 4 to recover viaduct timeline.
- **Milestone-Linked Invoicing**: Tie subcontractor disbursements strictly to verified Power BI milestone sign-offs.

---

## Slide 11: System Demonstration & Live Walkthrough
- *Demonstration of live slicer cross-filtering:*
  - Selecting "Infrastructure" filters to Metro, Promenade, and Flyover.
  - Selecting "Delayed" instantly highlights PRJ001, PRJ005, and PRJ007 across all visual cards.
  - Responsive conditional formatting flags overrun cells in soft red.

---

## Slide 12: Conclusion & Future Scope
- **Conclusion**: Power BI provides unprecedented visibility into construction operations, empowering engineers to detect cost leakages and schedule drift before they become catastrophic.
- **Future Enhancements**:
  - Live drone site survey integration via Power BI streaming datasets.
  - Predictive machine learning models for early cost overrun forecasting.
  - Integration with 4D Building Information Modeling (BIM).
- **Thank You!** Questions and Answers.
