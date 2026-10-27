# SATHYABAMA INSTITUTE OF SCIENCE AND TECHNOLOGY
### (DEEMED TO BE UNIVERSITY)
**Accredited with Grade "A++" by NAAC | Approved by AICTE**  
**Jeppiaar Nagar, Rajiv Gandhi Salai, Chennai - 600 119, Tamil Nadu, India**

---

### DEPARTMENT OF COMPUTER SCIENCE AND ENGINEERING
**Periodic Test - 1 (PT-1) Data Analytics Project Report**  
**Academic Year 2024–2025 | 3rd Year B.E. Computer Science Engineering**

---

# PROJECT TITLE:
# Construction Project Cost & Timeline Dashboard

---

**Student Submission Details:**  
- **Degree / Branch**: B.E. Computer Science and Engineering (3rd Year)  
- **Institution**: Sathyabama Institute of Science & Technology, Chennai  
- **Subject**: Data Analytics / Business Intelligence Laboratory  
- **Analytics Tool**: Microsoft Power BI Desktop & Power Query  

---

## TABLE OF CONTENTS
1. Title & Abstract
2. Introduction
3. Problem Statement
4. Objectives of the Study
5. Scope of the Project
6. Dataset Description
7. Data Collection & Synthesis Methodology
8. Data Cleaning & Power Query ETL
9. Data Preprocessing & Validation
10. Data Modeling & Star Schema Architecture
11. Power BI Implementation Architecture
12. DAX Measures & Mathematical Formulations
13. Key Performance Indicators (KPIs) Description
14. Dashboard Visual Design & UI Layout
15. Data Visualization Specifications
16. In-Depth Exploratory Data Analysis (EDA)
17. Key Empirical Findings
18. Actionable Project Management Insights
19. Strategic Engineering & Cost Recommendations
20. Conclusion
21. Future Scope & Enhancements

---

## 1. Abstract
In the infrastructure and civil construction sector, project failure is predominantly characterized by schedule overruns and budget creep. This project presents the design, development, and implementation of an end-to-end **Construction Project Cost & Timeline Dashboard** using **Microsoft Power BI Desktop**. Utilizing a realistic multi-table relational dataset based on infrastructure, commercial, and residential developments across the Chennai metropolitan area and Tamil Nadu, the study evaluates project health across 10 capital works valued at INR 715.00 Crores. 

Through robust Power Query ETL transformations, a 1-to-many Star Schema data model, and custom DAX measures, the dashboard tracks key metrics including Total Budget, Actual Cost, Cost Variance, Budget Deviation %, Project Completion %, Progress Gap / Delay %, and Milestone Completion Rate. Empirical analysis reveals a net portfolio cost overrun of +INR 39.23 Crores (+7.30%) and an average schedule delay of +6.70%, with specific bottlenecks in heavy infrastructure and commercial projects. The interactive dashboard provides executive decision-makers with drill-through visibility to mitigate cost escalations and realign milestone schedules.

---

## 2. Introduction
Modern civil engineering and construction management projects are notoriously vulnerable to operational uncertainties. Variations in material commodity pricing (cement, structural steel, ready-mix concrete), labor availability, regulatory approvals, monsoon disruptions, and unmapped underground utilities create severe variance between planned baselines and site reality. 

Traditional construction monitoring relies heavily on static, disconnected Excel spreadsheets, site logbooks, and sporadic contractor invoices. This fragmented approach delays variance detection until cost overruns become irreversible. Business Intelligence (BI) platforms like Microsoft Power BI revolutionize construction governance by synthesizing financial ledgers, engineering progress logs, and stage-gate milestones into unified, real-time interactive analytical models.

As 3rd-year Computer Science Engineering students, this project applies core principles of data warehousing, relational modeling, ETL pipelines, and analytical visualization to solve high-stakes construction engineering management problems.

---

## 3. Problem Statement
Project managers in large-scale infrastructure and real estate developments struggle to:
1. Concurrently track financial expenditure and physical on-site progress without latency.
2. Quantify the exact cost variance (`Actual Cost - Planned Cost`) across individual projects and monthly fiscal cycles.
3. Rapidly isolate which projects exhibit critical schedule gaps (`Planned Progress % - Actual Progress %`) before contractual liquidated damages apply.
4. Monitor milestone execution across contractors and stage-gates.
5. Provide C-suite stakeholders with an intuitive, unified visual dashboard that replaces 50-page text reports.

---

## 4. Objectives of the Study
The primary objectives of this PT-1 Data Analytics project are:
1. **Design a Realistic Data Pipeline**: Synthesize and clean a 4-table relational construction dataset representing INR 715 Cr of capital projects in the Chennai region.
2. **Build a Robust Relational Model**: Establish an optimized 1-to-many Star Schema in Power BI linking dimension tables to financial and progress fact tables.
3. **Formulate Comprehensive DAX Metrics**: Engineer 18+ syntactically correct DAX formulas to calculate Cost Variance, Budget Deviation %, Progress Gap, and Milestone Ratios without aggregation distortions.
4. **Develop an Executive Dashboard**: Create a modern, professional, responsive Power BI report featuring KPI cards, S-curve line charts, deviation bar charts, milestone donuts, and performance matrices.
5. **Derive Actionable Insights**: Translate raw numerical findings into concrete engineering recommendations to curtail cost overruns and recover delayed schedules.

---

## 5. Scope of the Project
- **Geographic & Sectoral Focus**: Infrastructure (Metro Rail, Flyovers, Coastal Promenade), Commercial (IT Tech Parks, Malls), Residential (Townships, Luxury Enclaves), and Industrial (EV Plants, Freight Hubs) across Chennai, Chengalpattu, Kanchipuram, and Tiruvallur.
- **Financial Dimension**: Budgets ranging from INR 35 Cr to INR 120 Cr denominated in Indian Rupees (INR / ₹).
- **Temporal Horizon**: 18 months of monthly tracking (January 2023 to June 2024).
- **Functional Scope**: Financial control, schedule tracking, milestone stage-gate delivery, and executive summary visualization.

---

## 6. Dataset Description
The underlying data model consists of four logically connected tables stored in `Construction_Project_Data.xlsx`:

1. **`Projects` (Dimension / Master Table)**: 10 records.
   - `Project ID` (Primary Key): Unique code (`PRJ001` to `PRJ010`).
   - `Project Name`: Full descriptive project title.
   - `Location`: Specific micro-market location.
   - `Project Manager`: Senior engineer in charge.
   - `Start Date` & `Planned End Date`: Contractual project duration.
   - `Budget`: Total sanctioned capital expenditure in INR.
   - `Project Type`: Infrastructure, Commercial, Residential, Industrial.
   - `Status`: High-level operational state (`In Progress`, `Completed`, `Delayed`).

2. **`Project_Cost` (Periodic Fact Table)**: 153 records.
   - `Project ID` (Foreign Key): Links to `Projects`.
   - `Date`: First day of each calendar month (`YYYY-MM-01`).
   - `Planned Cost`: Monthly baseline budget allocation.
   - `Actual Cost`: Monthly real expenditure booked on site.

3. **`Project_Progress` (Periodic Fact Table)**: 153 records.
   - `Project ID` (Foreign Key): Links to `Projects`.
   - `Date`: Monthly reporting timestamp.
   - `Planned Progress %`: Target cumulative engineering completion (0.00 to 1.00).
   - `Actual Progress %`: Verified cumulative physical completion on site (0.00 to 1.00).

4. **`Milestones` (Transaction / Stage-Gate Fact Table)**: 50 records.
   - `Milestone ID` (Primary Key): `MLS001` to `MLS050`.
   - `Project ID` (Foreign Key): Links to `Projects`.
   - `Milestone Name`: Specific engineering deliverable.
   - `Planned Date`: Contractual milestone target.
   - `Actual Date`: Real completion date (null if pending).
   - `Status`: `Completed`, `In Progress`, or `Delayed`.

---

## 7. Data Collection & Synthesis Methodology
To ensure high educational fidelity, real-world construction parameters were synthesized based on standard public works and CPWD (Central Public Works Department) schedule norms:
- **Financial Consistency**: Planned monthly disbursements were derived based on bell-curve and S-curve capital outlay profiles typical of civil works.
- **Controlled Variance**: Projects were intentionally assigned diverse performance profiles:
  - 2 Completed projects finished under budget (e.g., PRJ003 Marina Coastal Promenade achieved a 6.25% cost saving).
  - 3 Delayed projects suffered severe cost escalations (e.g., PRJ001 Chennai Metro at +16.61% overrun and PRJ005 Velachery Mall at +16.00% overrun).
  - 5 In-progress projects maintained controlled variations between +0.8% and +4.8%.
- **Chronological Coherence**: Milestone dates and monthly time-series data adhere strictly to calendar constraints without illogical time gaps or negative cost records.

---

## 8. Data Cleaning & Power Query ETL
Data ingestion was executed using Power Query within Power BI Desktop. The cleaning steps included:
1. **Source Parsing**: Excel sheets were loaded as distinct queries. First rows were promoted to headers using `Table.PromoteHeaders`.
2. **Explicit Data Typing**:
   - Currency fields were assigned `Int64.Type` to optimize VertiPaq columnar compression.
   - Progress fields were typed as `type number` (Double precision decimal) and set to Percentage display format.
   - Dates were explicitly converted from strings to `type date`.
3. **Handling Blanks in Actual Dates**: In the `Milestones` table, pending milestones have no completion date. Rather than inserting invalid placeholder dates (such as `1900-01-01`), `null` values were preserved to maintain accurate chronological integrity.
4. **Primary Key Deduplication**: Verified zero duplicate keys on `Projects[Project ID]` via `Table.Distinct`.

---

## 9. Data Preprocessing & Validation
Pre-load integrity scripts confirmed:
- Zero orphan foreign keys: All 153 cost entries, 153 progress entries, and 50 milestones correspond to valid records in `Projects`.
- Monotonic Progress Growth: Cumulative physical progress strictly increases over time without retrogressing.
- Upper Boundary Constraints: No progress metric exceeds 100% (1.0000).

---

## 10. Data Modeling & Star Schema Architecture
A star-like dimensional model was implemented to maximize query performance:

```
                  +---------------------------+
                  |         Projects          |
                  |     (Dimension Table)     |
                  | PK: Project ID            |
                  +---------------------------+
                        |       |       |
             1-to-many  |       |       |  1-to-many
              (1 : *)   |       |       |   (1 : *)
         +--------------+       |       +---------------+
         |                      |                       |
         v                      v                       v
+------------------+ +--------------------+ +--------------------+
|   Project_Cost   | |  Project_Progress  | |     Milestones     |
|   (Fact Table)   | |    (Fact Table)    | |    (Fact Table)    |
| FK: Project ID   | | FK: Project ID     | | FK: Project ID     |
+------------------+ +--------------------+ +--------------------+
```

- **Relationships**:
  - `Projects[Project ID]` (1) -> `Project_Cost[Project ID]` (*)
  - `Projects[Project ID]` (1) -> `Project_Progress[Project ID]` (*)
  - `Projects[Project ID]` (1) -> `Milestones[Project ID]` (*)
- **Cardinality**: `One-to-Many (1:*)`.
- **Cross-Filter Direction**: `Single` (Filters propagate downward from `Projects` to fact tables). This prevents circular filter dependencies and ambiguity.

---

## 11. Power BI Implementation Architecture
The Power BI application consists of three decoupled layers:
1. **Storage Layer (VertiPaq Engine)**: Highly compressed columnar in-memory database holding the 4 relational tables.
2. **Calculation Layer (DAX Formula Engine)**: Centralized `_Measures` table housing 18 business rules evaluated dynamically against visual coordinates.
3. **Visualization Layer**: Interactive report canvas featuring linked cross-filtering, slicers, cards, charts, and matrix controls.

---

## 12. DAX Measures & Mathematical Formulations

### Primary Measures & Definitions

1. **Total Budget**:
   $$\text{Total Budget} = \sum \text{Projects}[\text{Budget}]$$
   *Total sanctioned capital commitment across portfolio.*

2. **Planned Cost**:
   $$\text{Planned Cost} = \sum \text{Project\_Cost}[\text{Planned Cost}]$$
   *Cumulative baseline expenditure planned to date.*

3. **Actual Cost**:
   $$\text{Actual Cost} = \sum \text{Project\_Cost}[\text{Actual Cost}]$$
   *Total real capital spent on site.*

4. **Cost Variance**:
   $$\text{Cost Variance} = \text{Actual Cost} - \text{Planned Cost}$$
   *Positive indicates overrun; negative indicates savings.*

5. **Budget Deviation %**:
   $$\text{Budget Deviation \%} = \frac{\text{Cost Variance}}{\text{Planned Cost}} \times 100$$
   *Implemented via `DIVIDE([Cost Variance], [Planned Cost], 0)` for zero-safe division.*

6. **Project Completion %**:
   $$\text{Project Completion \%} = \max(\text{Project\_Progress}[\text{Actual Progress \%}])$$
   *Latest cumulative physical completion achieved on site.*

7. **Planned Progress %**:
   $$\text{Planned Progress \%} = \max(\text{Project\_Progress}[\text{Planned Progress \%}])$$
   *Expected cumulative progress as per schedule.*

8. **Progress Gap / Delay %**:
   $$\text{Progress Gap} = \text{Planned Progress \%} - \text{Project Completion \%}$$
   *Positive indicates schedule slippage / delay.*

9. **Average Project Completion %**:
   $$\text{Average Project Completion \%} = \frac{1}{N} \sum_{i=1}^N \text{Project Completion \%}_i$$
   *Implemented via `AVERAGEX(Projects, [Project Completion %])` to prevent monthly record count skew.*

10. **Milestone Completion %**:
    $$\text{Milestone Completion \%} = \frac{\text{Completed Milestones}}{\text{Total Milestones}} \times 100$$

---

## 13. Key Performance Indicators (KPIs) Description

The top banner of the dashboard exposes five critical executive KPIs:
1. **Total Budget (INR 715.00 Cr)**: Total capital under management.
2. **Actual Cost (INR 576.73 Cr)**: Cumulative capital expended to date.
3. **Cost Variance (+INR 39.23 Cr)**: Net portfolio cost overrun across all works.
4. **Average Project Completion % (83.70%)**: Portfolio physical progress.
5. **Budget Deviation % (+7.30%)**: Normalized portfolio financial overrun.

---

## 14. Dashboard Visual Design & UI Layout
Designed according to modern human-computer interaction (HCI) standards:
- **Dimensions**: 16:9 ratio (1280 x 720 px).
- **Banner**: Deep Navy Blue (`#1E3A8A`) conveying authority and institutional structure.
- **Card Containers**: White cards with 1px border (`#E2E8F0`) providing visual separation without visual clutter.
- **Color Semantics**: Consistent use of traffic-light indicators (Green = Favorable/On-track, Amber = Caution/In Progress, Red = Delayed/Over Budget).

---

## 15. Data Visualization Specifications

| Visual Name | Chart Type | X / Category Axis | Y / Metric Value | Key Purpose |
|---|---|---|---|---|
| Monthly Spend Trend | Line & Column | `Date` (Month) | Planned Cost & Actual Cost | Spot monthly budget burn vs planned allocation |
| Progress S-Curve | Line Chart | `Date` (Month) | Planned % vs Actual % | Visualize schedule divergence over time |
| Budget Deviation % | Bar Chart | `Project Name` | `Budget Deviation %` | Rank projects by cost overrun severity |
| Milestone Breakdown | Donut Chart | `Status` | `Total Milestones` | Stage-gate completion ratio (Completed/Pending/Delayed) |
| Project Matrix Table | Matrix / Table | `Project Name` | All 9 core metrics | Detailed operational audit for reviews |

---

## 16. In-Depth Exploratory Data Analysis (EDA)

### Project Performance Summary Table

| ID | Project Name | Type | Budget (₹ Cr) | Act Cost (₹ Cr) | Variance (₹ Cr) | Dev % | Act Prog % | Delay % | MS Comp % | Status |
|---|---|---|---|---|---|---|---|---|---|---|
| **PRJ001** | Chennai Metro Line 4 | Infra | 120.0 | 105.0 | +14.95 | +16.61% | 82.0% | 16.0% | 60.0% | Delayed |
| **PRJ002** | OMR Cyber Gateway | Commercial | 85.0 | 68.5 | +1.34 | +2.00% | 91.0% | 2.0% | 60.0% | In Progress |
| **PRJ003** | Marina Promenade | Infra | 35.0 | 32.8 | -2.19 | -6.25% | 100.0% | 0.0% | 100.0% | Completed |
| **PRJ004** | Guindy Green Tech | Commercial | 95.0 | 62.4 | +0.90 | +1.47% | 79.0% | 2.0% | 60.0% | In Progress |
| **PRJ005** | Velachery Mall | Commercial | 75.0 | 76.9 | +10.61 | +16.00% | 79.0% | 20.0% | 60.0% | Delayed |
| **PRJ006** | Tambaram Township | Residential | 60.0 | 32.5 | +0.30 | +0.93% | 72.0% | 2.0% | 60.0% | In Progress |
| **PRJ007** | Sriperumbudur EV Plant | Industrial | 110.0 | 107.5 | +13.97 | +14.94% | 87.0% | 13.0% | 60.0% | Delayed |
| **PRJ008** | Porur Grade Separator | Infra | 45.0 | 27.2 | +1.24 | +4.77% | 70.0% | 10.0% | 60.0% | In Progress |
| **PRJ009** | ECR Luxury Enclave | Residential | 40.0 | 37.9 | -2.11 | -5.27% | 100.0% | 0.0% | 100.0% | Completed |
| **PRJ010** | Poonamallee Freight Hub | Industrial | 50.0 | 26.0 | +0.21 | +0.83% | 77.0% | 2.0% | 60.0% | In Progress |

---

## 17. Key Empirical Findings
1. **Portfolio Overrun Driven by 3 Outliers**:
   - Out of INR 39.23 Cr in net cost overrun, **INR 39.53 Cr (99.2%)** is concentrated in just three projects:
     - `PRJ001` (Chennai Metro Line 4): +INR 14.95 Cr (+16.61%)
     - `PRJ007` (Sriperumbudur EV Plant): +INR 13.97 Cr (+14.94%)
     - `PRJ005` (Velachery Commercial Mall): +INR 10.61 Cr (+16.00%)
2. **Correlation Between Schedule Delays and Cost Creep**:
   - The three most delayed projects (`PRJ005` at 20% delay, `PRJ001` at 16% delay, and `PRJ007` at 13% delay) are exactly the three projects exhibiting double-digit budget deviation percentages. Extended site presence causes ballooning overhead, machinery rental, and labor costs.
3. **Completed Projects Demonstrated Cost Efficiency**:
   - Both completed projects (`PRJ003` Marina Coastal Promenade and `PRJ009` ECR Luxury Enclave) finished ahead of schedule and under budget (-6.25% and -5.27% variance respectively), proving that tight schedule adherence directly drives cost savings.
4. **Milestone Bottlenecks in MEP & Specialized Technology**:
   - 4 out of 50 milestones are actively delayed (`MLS004`, `MLS024`, `MLS034`, `MLS035`). Notably, `MLS034` (Automated Conveyor Cells) and `MLS035` (High-Tension Substation) in the EV Plant point to supply-chain customs bottlenecks for imported industrial machinery.

---

## 18. Actionable Project Management Insights
- **Immediate Cash Flow Intervention for PRJ005**: Velachery Mall has already spent INR 76.9 Cr against an original baseline budget of INR 75 Cr while physical completion is only 79%. Without design freeze, the final overrun will exceed 25%.
- **Fast-Track Viaduct Erection on Metro Line 4**: The delay in Milestone MLS004 is blocking downstream track laying and traction electrification.
- **Contractor Reallocation**: Lessons learned from the high-performing project managers of PRJ003 (Suresh Natarajan) and PRJ009 (Vigneshwaran S.) should be codified as standard operating procedures across lagging infrastructure sites.

---

## 19. Strategic Engineering & Cost Recommendations
1. **Implement Value Engineering & Scope Freezes**: Freeze further architectural modifications on PRJ005 (Velachery Mall) to stop retail fitout budget bleeding.
2. **Deploy Critical Path Method (CPM) Crash Scheduling**: Allocate additional concrete launching girders to PRJ001 (Metro Line 4) to compress the viaduct schedule before the Northeast monsoon.
3. **Pre-procurement of Specialized Industrial Equipment**: For industrial projects like PRJ007, import lead times for cleanrooms and automated conveyors must be locked with forward exchange contracts.
4. **Milestone-Linked Subcontractor Payments**: Mandate that sub-contractor progress claims are audited directly against Power BI milestone verification certificates.

---

## 20. Conclusion
This PT-1 project demonstrates the successful application of business analytics and data visualization to construction management. By transforming raw, multi-table operational data into an interactive Microsoft Power BI dashboard, complex metrics such as Cost Variance (+INR 39.23 Cr) and Progress Gap (+6.70%) are instantly accessible to leadership. The project fulfills all academic criteria for 3rd-year B.E. Computer Science Engineering, illustrating proficiency in database design, ETL pipelines, DAX formula engineering, and data storytelling.

---

## 21. Future Scope & Enhancements
1. **IoT Sensor & Drone Photogrammetry Integration**: Ingest real-time drone volumetric scans and CCTV feed metrics directly into Power BI via REST APIs.
2. **Machine Learning Predictive Forecasting**: Train Azure ML / Python regression models to predict final project cost overruns based on month-3 progress indicators.
3. **BIM (Building Information Modeling) 4D Sync**: Embed Autodesk BIM 3D models into Power BI using custom visuals to view color-coded 3D structural elements based on milestone status.
