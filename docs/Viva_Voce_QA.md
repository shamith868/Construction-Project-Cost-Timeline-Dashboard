# Viva Voce Preparation Guide (Questions & Answers)

**Subject**: Data Analytics / Power BI Lab  
**Course**: 3rd-Year B.E. Computer Science Engineering, Sathyabama Institute of Science & Technology  
**Project**: Construction Project Cost & Timeline Dashboard  

---

### Q1. Why did you choose Microsoft Power BI over tools like Excel or Tableau for this project?
**Answer**:  
While Excel is good for static spreadsheets, it struggles with automated relational modeling, multi-table relationships, and large time-series datasets. Tableau is strong in visualization, but Power BI offers the end-to-end integration of **Power Query (M-engine)** for ETL, the **VertiPaq engine** for in-memory columnar compression, and **DAX (Data Analysis Expressions)** for dynamic business calculations. Furthermore, Power BI is the enterprise standard across engineering corporations for interactive executive reporting.

---

### Q2. Why did you use multiple relational tables instead of merging everything into a single flat Excel sheet?
**Answer**:  
A single denormalized table introduces severe data redundancy, high memory consumption, and calculation anomalies (e.g., repeatedly duplicating the project's total budget on every monthly row would lead to massive sum inflation). By splitting the data into a **Dimension table (`Projects`)** and three **Fact tables (`Project_Cost`, `Project_Progress`, `Milestones`)**, we achieve:
1. Normalization (3NF principles).
2. Elimination of update anomalies.
3. Accurate aggregations via a Star Schema.

---

### Q3. Explain the Star Schema model you implemented. What is the cardinality and cross-filter direction?
**Answer**:  
We implemented a Star Schema where `Projects` acts as the central Dimension / Lookup table containing the primary key `Project ID`. The fact tables (`Project_Cost`, `Project_Progress`, `Milestones`) reference `Project ID` as a foreign key.  
- **Cardinality**: `1-to-Many (1:*)` from `Projects` to each fact table.  
- **Cross-filter direction**: `Single` (from 1 to *). This ensures filters selected on project attributes propagate naturally downward to filter costs, progress, and milestones without causing ambiguous filtering paths.

---

### Q4. What is the difference between Power Query (M) and DAX?
**Answer**:  
- **Power Query (M)**: Operates at the **Data Ingestion / ETL stage**. It cleans, transforms, pivots, types, and loads data into memory. It is evaluated during data refresh.
- **DAX (Data Analysis Expressions)**: Operates at the **Data Modeling / Reporting stage**. It computes dynamic calculations on the fly in response to user clicks, slicers, and visual context.

---

### Q5. What is the difference between a Calculated Column and a DAX Measure?
**Answer**:  
- **Calculated Column**: Evaluated row-by-row during data load and stored in RAM. It consumes file space and memory.
- **DAX Measure**: Computed on demand based on the active **Filter Context** of the visual. It uses zero persistent storage and is computationally efficient. All our KPIs (Cost Variance, Budget Deviation %, etc.) are created as DAX Measures.

---

### Q6. What is a KPI, and which KPIs are highlighted in your dashboard?
**Answer**:  
A Key Performance Indicator (KPI) is a quantifiable metric used to measure progress toward strategic goals. In our construction dashboard, the top 5 KPIs are:
1. **Total Budget**: INR 715.00 Cr (Sanctioned portfolio budget).
2. **Actual Cost**: INR 576.73 Cr (Total spend to date).
3. **Cost Variance**: +INR 39.23 Cr (Total overrun).
4. **Average Project Completion %**: 83.70% (Overall physical work completed).
5. **Budget Deviation %**: +7.30% (Normalized financial overrun rate).

---

### Q7. How is Cost Variance calculated, and what does the sign of the result indicate?
**Answer**:  
$$\text{Cost Variance} = \text{Actual Cost} - \text{Planned Cost}$$
In our DAX formula: `[Actual Cost] - [Planned Cost]`.  
- **Positive (+)** indicates an **Unfavorable / Over Budget** situation (the project spent more money than scheduled).
- **Negative (-)** indicates a **Favorable / Under Budget** situation (the project achieved savings).

---

### Q8. Why did you use `DIVIDE()` instead of the standard `/` operator in your Budget Deviation % formula?
**Answer**:  
In DAX, `[Cost Variance] / [Planned Cost]` will return a `#DIV/0!` error or infinity if `Planned Cost` is zero or blank. The `DIVIDE([Cost Variance], [Planned Cost], 0)` function handles zero denominators gracefully by returning the safe alternate result `0`.

---

### Q9. How did you calculate Average Project Completion % without skewing the result?
**Answer**:  
If we simply used `AVERAGE(Project_Progress[Actual Progress %])`, Power BI would average all 153 monthly historical rows, which would reflect the mid-point of history (~45%) rather than current completion!  
To solve this, we used:
```dax
Average Project Completion % = AVERAGEX(Projects, [Project Completion %])
```
where `[Project Completion %] = MAX(Project_Progress[Actual Progress %])`. This iterates over the 10 distinct projects, extracts the latest physical progress of each, and averages those 10 values, correctly yielding **83.70%**.

---

### Q10. What is Progress Gap, and how does it measure project delay?
**Answer**:  
$$\text{Progress Gap} = \text{Planned Progress \%} - \text{Project Completion \%}$$
If Planned Progress is 98% but Actual Progress is only 82%, the Progress Gap is **+16%**. A positive gap directly indicates that physical site execution is lagging behind the engineering master schedule.

---

### Q11. What is the role of Slicers in Power BI?
**Answer**:  
Slicers are visual filter controls placed on the canvas. When a user clicks a slicer item (e.g., Project Type = "Infrastructure" or Status = "Delayed"), Power BI immediately modifies the **Filter Context** of every connected visual on the canvas, recalculating all DAX measures instantaneously for that specific subset.

---

### Q12. What are the key empirical insights revealed by your dataset?
**Answer**:  
1. **Concentration of Overruns**: 99.2% of the entire portfolio cost overrun (+INR 39.23 Cr) is caused by just three projects: Chennai Metro Line 4 (+INR 14.95 Cr), Sriperumbudur EV Plant (+INR 13.97 Cr), and Velachery Mall (+INR 10.61 Cr).
2. **Correlation of Delay with Cost**: The projects with the highest schedule delay (Velachery Mall at 20% delay, Metro at 16% delay) had the highest budget overrun percentages (+16%).
3. **Efficiency in Completed Works**: Marina Coastal Promenade and ECR Luxury Enclave were completed with ~5% to 6% budget savings.

---

### Q13. How does this dashboard assist a real Construction Project Manager?
**Answer**:  
A project manager can:
1. Conduct monthly cost control reviews in minutes rather than sifting through spreadsheets.
2. Quickly pinpoint which stage-gate milestones are delayed (e.g., automated conveyor installation).
3. Enforce design freezes on projects like Velachery Mall before costs spiral further.
4. Tie subcontractor billing approvals directly to verified Power BI milestone completions.

---

### Q14. What happens if a milestone has no Actual Date in your data?
**Answer**:  
In civil construction, pending or future milestones have not yet been achieved. In our dataset and Power Query transformation, these cells are preserved as `null`. Power BI correctly handles null dates without crashing, and our DAX measure `[Pending Milestones]` identifies them by filtering on `Status = "In Progress"`.

---

### Q15. If a new month of construction data is recorded, how is the dashboard updated?
**Answer**:  
Because of Power BI's automated ETL pipeline, the user simply appends the new monthly rows to `Construction_Project_Data.xlsx` and clicks the **Refresh** button on the Home ribbon. Power Query automatically re-applies all transformations, re-evaluates the DAX measures, and updates all visuals immediately.
