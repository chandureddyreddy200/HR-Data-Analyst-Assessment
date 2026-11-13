README - Reproducible work (Deliverable E)
Saichandana Reddyreddy, HR Data Analyst assessment

WHAT IS IN THIS FOLDER
- Assessment_Package\Data\ : the 7 original CSV files, never edited.
- HR_Cleaning.xlsx : all cleaning and calculation steps, recorded in Power Query
  (Data > Queries & Connections; each query's Applied Steps show every change).
  Sheets: DQ_Log (every data-quality check), Key_Results, Stage_Summary, Path_Summary.
- Processed_Data\hires.csv : one row per completed hire (271 rows).
- Processed_Data\stage_activities.csv : one row per hiring step (4,877 rows).

HOW TO RE-RUN
1. Open HR_Cleaning.xlsx.
2. If the files are in a new location: Data > Get Data > Data Source Settings >
   Change Source, point each file to Assessment_Package\Data, and check that
   File Origin is 65001: Unicode (UTF-8).
3. Data > Refresh All. Expected: Hires 271 rows, 36.5% under 90 days, median 95 days.

KEY DEFINITIONS
- Days to fill = Onboarding_Completion_Date - earliest Posted_Date for the job.
- Met target = fewer than 90 days.
- Step time = Completed_Date - Assigned_Date (8 negative values blanked).

TABLEAU (HR_Recruitment_Dashboard.twbx)
Data sources: hires.csv, stage_activities.csv, and the original Process_Standards.csv.
Blending: target days from Process_Standards are linked to stage_activities on Stage Name.
Filters Administration and Job Family apply to all related data sources.

Parameter
- Target Days: integer, 60 to 150, step 5, default 90.

Calculated fields (hires)
- % Met Target = SUM(IF [Days To Fill] < [Target Days] THEN 1 ELSE 0 END) / COUNT([Offer ID])
- Hires = COUNTD([Offer ID])
- Target Days Label = MIN([Target Days])

Bottleneck chart (stage_activities)
- Bars = AVG([Elapsed Days Valid]) by Stage Name, filtered to exclude the Pre-posting and Closure phases.
- Black line = reference line on [Target Calendar Days] from Process_Standards (blended).