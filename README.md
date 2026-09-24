# bay-area-hospital-2026-revenue-cycle-dashboard
Data visualization of revenue, occupancy rates, and discharge rates, by county
# Bay Area Revenue Cycle Ledger

An interactive dashboard comparing hospital revenue, payer mix, occupancy, and operating margin across 72 Bay Area hospitals in 9 counties, built from public California hospital financial data.

**[Live Demo](https://isaacryn.github.io/bay-area-hospital-2026-revenue-cycle-dashboard/)** · Analysis notebook: (https://www.kaggle.com/code/isaacnguyen/data-analysis-of-ca-hospital-revenue-in-2026)

<img width="2513" height="1194" alt="Interactive3D" src="https://github.com/user-attachments/assets/d85a8a46-c070-4202-8e60-e709b17a1c2d" />



## Key Findings

- **County averages can hide a struggling hospital.** San Mateo County posts a combined operating margin of **+0.22%**, but San Mateo Medical Center runs at **−20.6%**. Mills-Peninsula (+11.8%) and Kaiser South San Francisco (+8.3%) offset it in the county total.
- **Two losses, two different causes.**
  - San Mateo Medical Center's deficit comes from its payer mix, not its efficiency: **87% of its payer mix is Medi-Cal**, which reimburses below cost. Its occupancy is 94%.
  - AHMC Seton Medical Center's deficit (−39.9%) comes from underused capacity: 377 staffed beds at **~40% occupancy**.
- **Napa County is a statistical outlier** at −2.34 standard deviations from the Bay Area mean margin. The cause is Napa State Hospital's state-funded structure, not a data error.

## Dashboard Views

| View | What it shows |
|---|---|
| **Facility Map** | 3D bar chart of every hospital (three.js). Bar height can show revenue per discharge or occupancy rate; color shows loss / breakeven / surplus. |
| **County Comparison** | Operating margin by county, payer mix by county, and margin vs. discharges per 1,000 residents (Chart.js). |
| **Methodology** | Step-by-step documentation of the data pipeline and the decisions behind it. |

## Data Sources

| File | Source |
|---|---|
| `data/2026_q1.xlsx` | CA Dept. of Health Care Access and Information (HCAI), Hospital Quarterly Financial & Utilization Report, Q1 2026 |
| `data/hosp25_util_data_prelim.xlsx` | HCAI Hospital Annual Utilization Report, 2025 (preliminary) |
| `data/PopulationEstimate_July_2020-2025_Feb26_w.xlsx` | CA Dept. of Finance, county population estimates |
| `data/bay_area_dashboard_data.json` | Final processed dataset used by the dashboard (72 facilities, 9 counties) |

All source data is public.

## Methodology

1. **Standardized three sources.** Each file had different column names, header structures, and shapes.
2. **Fixed three data-quality bugs before merging.** Multi-word county names were split across two rows in the population file; one county's year was stored as text, which silently dropped rows; stray whitespace was left from rejoining names. Each fix was checked before and after.
3. **Validated join keys first.** Confirmed facility IDs matched exactly across both HCAI files, and county names matched the population file, before any merge.
4. **Scoped to the Bay Area.** A wider Northern California scope would have included 9 counties with only one hospital each, turning a county comparison into single-facility numbers. All 9 Bay Area counties have multiple hospitals.
5. **Applied a volume floor.** Excluded 3 facilities under 50 discharges per quarter from ratio comparisons. One 5-discharge specialty hospital produced a $940K revenue-per-discharge figure driven by sample size.
6. **Summed dollars before computing ratios.** County margins are total revenue minus total expense, divided by total revenue. They are not averages of hospital margins, which would give small hospitals the same weight as large ones.
7. **Investigated outliers before reporting them.** Flagged by z-score, then traced to a root cause.
8. **Exported a lean dataset.** Reduced the 156-column working table to the fields the dashboard uses.

## Data Notes

Values reported as filed by hospitals and kept as-is:
- Stanford Health Care reports more staffed beds (750) than licensed beds (622).
- Kaiser Vallejo Rehabilitation Center reports a Medi-Cal share of −0.64%, likely a prior-period adjustment.
- Payer shares cover Medicare, Medi-Cal, and commercial only, so some facilities, especially psychiatric facilities, sum well below 100%. The remainder falls in other payer categories.
- The 2025 annual utilization file is preliminary and may be revised by HCAI.

## Tech Stack

Python (pandas) in Jupyter/Kaggle for cleaning, joining, and aggregation · three.js for the 3D facility map · Chart.js for county charts · a single self-contained HTML file with embedded data.

## Author

Isaac Nguyen · [GitHub](https://github.com/isaacryn)
