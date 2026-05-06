# Tableau Dashboard Specification — `radiology_dashboard.twbx`

## Data source

Single connection to `data/processed_output.csv` (output of the KNIME workflow / NLP notebook). Fields:

| Field | Type | Notes |
|-------|------|-------|
| `study_id` | String | Primary key |
| `case_type` | String | `routine`, `complex`, `review` |
| `ai_assisted` | Boolean (0/1) | Whether the read was AI-assisted |
| `turnaround_minutes` | Number | Time from study available to final read |
| `urgency_score` | Number | 0.0 – 1.0 |
| `urgent_flag` | Boolean (0/1) | `urgency_score >= 0.65` |

## Sheets

1. **Turnaround by case type (AI vs. unassisted)** — clustered bar chart, mean `turnaround_minutes` split by `ai_assisted`.
2. **Efficiency lift by case type** — calculated field `(unassisted_mean - ai_mean) / unassisted_mean`; bar chart.
3. **Urgency volume over time** — line chart of urgent-flag count by week (uses `study_id` proxy ordering when no timestamp available).
4. **Distribution of urgency scores** — histogram, 20 bins.
5. **KPI tiles** — total studies, percent AI-assisted, percent urgent, mean turnaround.

## Dashboard layout

- Top row: KPI tiles (4 across).
- Middle row: turnaround comparison + efficiency lift, side-by-side.
- Bottom row: urgency volume over time (full width) with histogram inset.
- Filters (right rail): `case_type`, `ai_assisted`, `urgent_flag`.

## Headline numbers

These match the README key result and should reproduce when the workbook is rebuilt against the full 11,200-record MIMIC-CXR extract:

- Complex cases: **34%** efficiency improvement.
- Routine AI-assisted: **29%**.
- Review/QA: **23%**.
