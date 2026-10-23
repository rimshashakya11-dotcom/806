# SmartCare Power BI Build Guide

Oct 12, 2026 · @Rimsha Shakya

This is the complete build script for a seven-page Power BI report that mirrors most of the SmartCare web console. You need three files from the `powerbi/` folder of SmartCare\_Code: `SmartCare_DAX.dax` holds every table, column, measure and the RLS rules; `SmartCare_theme.json` holds the emerald, ivory and gold theme; and `SmartCare_PowerBI_Build_Script.md` is this script. The build takes about 3–4 hours.

## 0. Before you start

1. Run `python smartcare_analysis.py` and then `python prepare_powerbi.py`. This writes seven CSV files to `outputs/powerbi/`.
2. In Power BI Desktop, go to **File > Options > Current file > Data load** and switch off *Auto date/time*.
3. Go to **View > Themes > Browse for themes** and choose `SmartCare_theme.json`.
4. Go to **Format page > Canvas settings** and set 16:9, 1280 × 720. Every page fits one screen with no scrolling.

## 1. Load and model the data

| File | Rename table to | Rows | Purpose |
| --- | --- | --- | --- |
| powerbi\_patients.csv | Patients | 50 | Measures, flags, tier, segment, readiness and its points, needs-action fields, gaps to target |
| powerbi\_risk\_factors.csv | RiskFactors | 350 | Patient × flag, for the prevalence bar chart |
| powerbi\_correlations.csv | Correlations | 121 | Long-format Spearman matrix, for the heatmap |
| powerbi\_care\_plan.csv | CarePlan | 129 | Patient × suggestion: priority, area, current → target, action, owner, timeframe |
| powerbi\_escalation.csv | Escalation | 11 | Agreed escalation steps per alert type |
| powerbi\_bands.csv | Bands | 6 | Guideline bands and their sources |
| powerbi\_rls\_users.csv | RLSUsers | 5 | Demo users for dynamic row-level security |

1. Load each file with **Get data > Text/CSV > Load**.
2. Check the types in Power Query:
   - IDs and categories are *Text*.
   - SleepDuration\_Hours is *Decimal*.
   - F\_ flags are *Whole number*.

   Then click **Close & Apply**.
3. In Model view, create these relationships:
   - `Patients[PatientID]` 1 → \* `RiskFactors[PatientID]`
   - `Patients[PatientID]` 1 → \* `CarePlan[PatientID]`

   Both are single direction. Leave Escalation, Bands and RLSUsers unrelated.
4. Set sort orders with **Column tools > Sort by column**:
   - RiskTier by RiskTierOrder
   - AgeGroup by AgeGroupOrder
   - ReadinessBand by ReadinessBandOrder
   - CarePlan\[Priority\] by PriorityOrder
5. Create an empty `_Measures` table with **Enter data**.
6. Paste the code from `SmartCare_DAX.dax` in order:
   - **Section A:** calculated tables, using **New table**. These are the what-if targets, suppression threshold, Axis X/Y, Compare A/B, Compare Measures and Pages.
   - **Section B:** calculated columns on Patients.
   - **Sections C–I:** measures, using **New measure**, placed in `_Measures`.
7. Sort Compare Measures, Axis X and Axis Y by their Order columns.
8. Hide the helper columns from report view.

**Check the model:**

- Patients (n) = 50
- Avg Readiness = 66.9
- Needs Action (n) = 10
- High Risk (n) = 8

## 2. Shared layout (build once, copy to every page)

- **Left navigation rail** (dark emerald #0F3D2E): **Insert > Buttons > Navigator > Page navigator**, vertical, ivory text, gold #B08D57 when selected.
- **Top bar:** a page title, a card showing `[Selection Text]` so users always see the active filters, and a **Reset filters** button.
- **Filter strip:** five slicers for Gender, AgeGroup, RiskTier, Segment and ActivityLevel. Use **View > Sync slicers** to sync them across Needs action, Overview and Insights.
- **Accessibility mode** (as in the console):
  1. Duplicate the page with fonts 2 pt larger and stronger borders.
  2. Add an "Aa Larger text" bookmark button.
  3. Add alt text on every visual.
  4. Set the tab order in the Selection pane.

## 3. Page 1: Needs action (clinician landing page)

| Visual | Fields | Settings |
| --- | --- | --- |
| 4 KPI cards | Needs Action (n), Urgent (n), Fall Alerts (n), ACC Referral Eligible (n) | Georgia 28 pt; accent from fx High Risk KPI Colour |
| Work-queue table | PatientID, ActionPriority, ActionReason, RiskTier, RiskScore, Readiness | Filter NeedsAction = 1; Urgent first; RiskTier background from field Tier Colour |
| Escalation card | Escalation Text | Updates when a queue row is clicked |
| Care-plan table | CarePlan Priority, Area, Current, Target, Owner, Timeframe | Priority background from fx Priority Colour |

Use **Edit interactions** so the queue filters the escalation and care-plan visuals.

## 4. Page 2: Overview (the visuals the brief requires)

| Visual | Fields | Notes |
| --- | --- | --- |
| KPI strip (6 cards) | Patients Shown, High Risk %, Avg Readiness (subtitle Readiness vs Cohort), Avg Sleep h, Avg Stress, Avg Steps | Small groups go blank and the Suppression Note card appears |
| Line chart | X Age or AgeGroup; Y Avg Sleep h, Avg Stress (secondary axis) | Trends across age |
| Donut | Legend RiskTier; Values Patients (n) | High #B3261E, Moderate #C98A1B, Low #1E7B4F |
| Bar chart | RiskFactors RiskFactor; Risk Factor % | Sorted descending, labels as % |
| Clustered column | AgeGroup × Gender; Patients (n) |  |
| Heatmap (matrix) | Rows Measure1, columns Measure2, value Spearman (static) | Background gradient −1 red, 0 ivory, +1 emerald |
| Scatter | Sleep vs Stress, details PatientID, legend RiskTier | Trend line; constant lines at 7 h and stress 70 |

**Report-page tooltip:**

1. Create a page called "Tip – Patient" at 320 × 240 and set **Allow use as tooltip**.
2. Add the Patient Header card and four mini cards.
3. Set the scatter's tooltip type to **Report page**.

## 5. Page 3: Patient record (drill-through)

This replaces "click a patient code" in the console.

1. Add PatientID to the **Drill through** well. Users right-click any patient and choose **Drill through > Patient record**, and the Back button is added automatically.
2. Add a header card with Patient Header in Georgia 18 pt.
3. Add eight vital tiles: HR avg, HR max (with HRmaxPredicted as subtitle), Sleep h, Sleep quality, Stress, Steps, Readiness and Risk score. Set the backgrounds from the Sleep, Stress, Steps and HR Status Colour measures.
4. Add a Readiness gauge from 0 to 100 with a target of 70.
5. Add bookmarked tabs. Put four layers in the Selection pane, create one bookmark per layer, and group four buttons as a tab bar:
   - **Summary:** Risk Ledger (the points ledger), Alignment Check and Readiness Breakdown.
   - **Measurements:** a clustered bar of Compare Measures with Measure Value For (Patient) and Cohort Avg Value, plus the Bands table.
   - **Care plan:** Care Plan Text, Strengths and ACC Referral Text.
   - **Readings:** a placeholder note that BP, weight, height, ECG and SpO₂ are not in the case dataset.

## 6. Page 4: Compare two patients

1. Add single-select dropdown slicers for Compare A \[Patient A\] and Compare B \[Patient B\].
2. Add a title card using Compare Title.
3. Build a butterfly chart: a clustered bar with Compare Measures on the axis and the values A Percentile (mirror) and B Percentile. Set the X axis from −100 to 100 and the format string to `0;0`.
4. Add a table with Measure, A Value, B Value, Cohort Avg Value and Better Of Two.
5. Add Axis X and Axis Y slicers and a scatter of X Value against Y Value, with PatientID as details and Scatter Title as the title. The live Spearman ρ updates with every filter.

## 7. Page 5: Care plan & what-if simulator

1. Add sliders for Target Sleep, Target Steps and Target Stress, or create them with **Modeling > New parameter > Numeric range**.
2. Add cards: High Risk (n) → Projected High (n), High Risk Avoided (n), Avg Readiness → Projected Readiness, and Readiness Gain.
3. Add a What-if Summary card, which writes one plain-English sentence.
4. Add a patient table with RiskTier, Projected Tier and data bars on SleepGap\_h, StepsGap and StressGap.
5. Add a care-plan matrix with CarePlan Area > Action as rows and Suggestions (n), Urgent Suggestions (n) and Patients With Plan (n) as values.

**Expected check** with targets of 7 h sleep, 7,500 steps and stress 50:

- High risk goes from 8 to 1.
- Average readiness goes from 66.9 to 80.2.

## 8. Page 6: Insights (analyst, aggregate only)

- **Finding cards:** Key Finding 1 to 4.
- **Key influencers:** analyse RiskTier = High, explained by sleep, stress, steps, HR, age and activity.
- **Decomposition tree:** Patients (n) by RiskTier → AgeGroup → Gender → Segment → ActivityLevel.
- **Group summary matrix:** Segment with an AgeGroup drill-down, showing Patients Shown, Avg Readiness, Avg Sleep h, Avg Stress and High Risk %. Small groups appear blank.
- **Readiness histogram:** bins of size 10.
- **Slicer:** Suppression Threshold, as an admin setting with a default of 5.

## 9. Page 7: Privacy & model validation

- **Privacy statement text box:** pseudonymised IDs, consent, Privacy Act 2020 / HIPC 2020 rule 5, small-cell suppression, and no data leaving the tenant.
- **Vendor vs Readiness r card** (r = 0.11) with Key Finding 4. Together they show why the vendor score is not used for decisions.
- **Bands table** as a transparent measure dictionary, plus a Smart narrative over the KPIs.

## 10. Row-level security (replaces the console sign-in roles)

1. Go to **Modeling > Manage roles**.
2. Create three roles:
   - **Admin:** no filter.
   - **Clinician (scoped):** apply the Section J DAX on Patients. It uses USERPRINCIPALNAME() with RLSUsers.
   - **Analyst:** no row filter. Hide the Patient record page and drill-through, and hide columns with Object-Level Security in Tabular Editor.
3. Test with **View as > Other user**: enter nurse.south@smartcare.nz with Clinician (scoped). Only patients aged 70–79 should appear.
4. Once the report is published, Microsoft Entra ID handles sign-in, MFA and password policy.

## 11. Final polish checklist

- [ ] Dynamic titles (Overview Title, Compare Title, Scatter Title)
- [ ] Alt text on every visual, and colour never the only signal
- [ ] Performance analyzer shows every page rendering in under 1 second
- [ ] Bookmarks: Default view, Accessibility mode, High-risk focus
- [ ] Tooltip page hidden and objects locked
- [ ] Saved as SmartCare\_Dashboard.pbix and a PDF exported for the report appendix

## 12. Console feature → Power BI coverage

| Console feature | Power BI equivalent | Coverage |
| --- | --- | --- |
| Five filters, KPI strip, required charts | Synced slicers, cards, line, pie, bar, heatmap, scatter | Full |
| Click patient code → full record | Drill-through page + report-page tooltip | Full |
| Points ledger, alignment check, readiness | Risk Ledger, Alignment Check, Readiness Breakdown | Full |
| Compare two patients | Compare A/B tables, percentile butterfly | Full |
| Measure vs measure with live Spearman ρ | Axis X/Y pickers + Spearman rho (X,Y) | Full |
| Care suggestions, strengths, ACC falls referral | CarePlan table and text measures | Full (from the dataset) |
| What-if simulator | Numeric parameters + projected measures | Full |
| Needs action + escalation pathway | Filtered queue + Escalation Text | View only |
| Admin / clinician / analyst roles | RLS + OLS with Entra ID sign-in | Full when published |
| Small-cell suppression | Is Suppressed pattern + threshold slicer | Full |
| Insights | Key influencers, decomposition tree, finding cards | Full |
| Accessibility mode | Larger-text bookmark, alt text, tab order | Full |
| Editing records, readings, passwords, symptom check-in, whānau sharing, edit audit log | Power BI is read-only; these need Power Apps or the web console | Web console only |
