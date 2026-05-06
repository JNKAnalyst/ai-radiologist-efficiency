# Power BI — Real-time Urgency Triage Dashboard

The Power BI dashboard provides near-real-time triage of incoming chest X-ray reports, ranking studies by `urgency_score` so radiologists can pull the highest-risk reads first.

## Data source

`data/processed_output.csv` (or a live SQL view, when wired into a hospital data warehouse). The notebook writes this CSV; in production the same schema is materialized as a view.

## DAX measures

```DAX
Urgent Count        = CALCULATE(COUNTROWS('reports'), 'reports'[urgent_flag] = 1)
Pct Urgent          = DIVIDE([Urgent Count], COUNTROWS('reports'))
Avg Turnaround      = AVERAGE('reports'[turnaround_minutes])
AI Lift %           =
VAR ai_mean   = CALCULATE(AVERAGE('reports'[turnaround_minutes]), 'reports'[ai_assisted] = 1)
VAR base_mean = CALCULATE(AVERAGE('reports'[turnaround_minutes]), 'reports'[ai_assisted] = 0)
RETURN DIVIDE(base_mean - ai_mean, base_mean)
```

## Pages

1. **Triage queue** — table of studies sorted by `urgency_score` desc, with conditional formatting on the score column. Filters: `case_type`, `ai_assisted`.
2. **Throughput** — KPI cards for `Avg Turnaround`, `AI Lift %`, `Pct Urgent`; line chart of urgent-count by hour-of-day.
3. **Quality / review** — scatter of `turnaround_minutes` vs. `urgency_score` with `case_type` color encoding.

## Refresh

In production, the dataset is set to incremental refresh every 5 minutes against the warehouse view. For the offline demo against the CSV, refresh is manual.
