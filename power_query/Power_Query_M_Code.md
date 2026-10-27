# Power Query Transformation & M-Code Reference

**Project**: Construction Project Cost & Timeline Dashboard  
**Source File**: `Construction_Project_Data.xlsx`  
**Tool**: Power Query Editor in Microsoft Power BI Desktop  

---

## 1. Power Query Cleaning Workflow

When ingesting real-world construction data, discrepancies such as unexpected blank cells, mixed date formats, and numerical values imported as text must be corrected. The Power Query ETL pipeline follows these standard steps:

1. **Source Connection**: Ingest from `Construction_Project_Data.xlsx`.
2. **Promote Headers**: Ensure the first row of each sheet becomes the column header.
3. **Data Type Casting**:
   - `Project ID`, `Project Name`, `Location`, `Project Manager`, `Project Type`, `Status`: `Text` (`type text`).
   - `Budget`, `Planned Cost`, `Actual Cost`: `Whole Number` (`Int64.Type`) or `Currency`.
   - `Planned Progress %`, `Actual Progress %`: `Decimal Number` (`type number`).
   - `Start Date`, `Planned End Date`, `Date`, `Planned Date`, `Actual Date`: `Date` (`type date`).
4. **Handling Blanks**:
   - In `Milestones[Actual Date]`, pending milestones legitimately have no actual completion date. Power Query preserves these as `null`. No artificial dummy date is injected, avoiding distorted timeline averages.
5. **Deduplication Check**:
   - Applied `Table.Distinct(..., {"Project ID"})` on the `Projects` query to guarantee primary key uniqueness.

---

## 2. Advanced Editor M-Code for Each Query

You can inspect or paste these exact M-code scripts into the **Advanced Editor** in Power Query:

### Query 1: `Projects`
```powerquery
let
    Source = Excel.Workbook(File.Contents("C:\\Users\\Admin\\.gemini\\antigravity\\scratch\\Construction_Project_Dashboard\\data\\Construction_Project_Data.xlsx"), null, true),
    Projects_Sheet = Source{[Item="Projects",Kind="Sheet"]}[Data],
    #"Promoted Headers" = Table.PromoteHeaders(Projects_Sheet, [PromoteAllScalars=true]),
    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{
        {"Project ID", type text}, 
        {"Project Name", type text}, 
        {"Location", type text}, 
        {"Project Manager", type text}, 
        {"Start Date", type date}, 
        {"Planned End Date", type date}, 
        {"Budget", Int64.Type}, 
        {"Project Type", type text}, 
        {"Status", type text}
    }),
    #"Removed Duplicates" = Table.Distinct(#"Changed Type", {"Project ID"})
in
    #"Removed Duplicates"
```

---

### Query 2: `Project_Cost`
```powerquery
let
    Source = Excel.Workbook(File.Contents("C:\\Users\\Admin\\.gemini\\antigravity\\scratch\\Construction_Project_Dashboard\\data\\Construction_Project_Data.xlsx"), null, true),
    Project_Cost_Sheet = Source{[Item="Project_Cost",Kind="Sheet"]}[Data],
    #"Promoted Headers" = Table.PromoteHeaders(Project_Cost_Sheet, [PromoteAllScalars=true]),
    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{
        {"Project ID", type text}, 
        {"Date", type date}, 
        {"Planned Cost", Int64.Type}, 
        {"Actual Cost", Int64.Type}
    })
in
    #"Changed Type"
```

---

### Query 3: `Project_Progress`
```powerquery
let
    Source = Excel.Workbook(File.Contents("C:\\Users\\Admin\\.gemini\\antigravity\\scratch\\Construction_Project_Dashboard\\data\\Construction_Project_Data.xlsx"), null, true),
    Project_Progress_Sheet = Source{[Item="Project_Progress",Kind="Sheet"]}[Data],
    #"Promoted Headers" = Table.PromoteHeaders(Project_Progress_Sheet, [PromoteAllScalars=true]),
    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{
        {"Project ID", type text}, 
        {"Date", type date}, 
        {"Planned Progress %", type number}, 
        {"Actual Progress %", type number}
    })
in
    #"Changed Type"
```

---

### Query 4: `Milestones`
```powerquery
let
    Source = Excel.Workbook(File.Contents("C:\\Users\\Admin\\.gemini\\antigravity\\scratch\\Construction_Project_Dashboard\\data\\Construction_Project_Data.xlsx"), null, true),
    Milestones_Sheet = Source{[Item="Milestones",Kind="Sheet"]}[Data],
    #"Promoted Headers" = Table.PromoteHeaders(Milestones_Sheet, [PromoteAllScalars=true]),
    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{
        {"Milestone ID", type text}, 
        {"Project ID", type text}, 
        {"Milestone Name", type text}, 
        {"Planned Date", type date}, 
        {"Actual Date", type date}, 
        {"Status", type text}
    })
in
    #"Changed Type"
```

---

## 3. Creating the `_Measures` Home Table in Power Query / Power BI

To keep DAX measures cleanly isolated from data tables:
1. In Power BI Desktop, click **Enter Data** on the **Home** ribbon.
2. Name the table `_Measures`.
3. Leave `Column1` empty and click **Load**.
4. Once your DAX measures are added to `_Measures`, right-click `Column1` and select **Hide in report view**. Power BI will automatically display `_Measures` at the very top of your Data pane with a calculator icon.
