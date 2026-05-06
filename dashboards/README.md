# Dashboards

This directory documents the Tableau and Power BI dashboards built on top of the NLP pipeline output (`data/processed_output.csv`).

## Files

- `radiology_dashboard.twbx` — Tableau packaged workbook. Not committed because it bundles a derivative extract of the MIMIC-CXR data, which is governed by a PhysioNet data use agreement. The structure is documented in `tableau_dashboard_spec.md` so the workbook can be rebuilt locally.
- `powerbi_urgency_triage_spec.md` — Specification for the Power BI real-time urgency-triage dashboard.

## Rebuilding the Tableau workbook

1. Run the KNIME workflow (or the Python notebook) to produce `data/processed_output.csv`.
2. Open Tableau Desktop or Tableau Public, connect to that CSV.
3. Build the sheets and dashboard layout described in `tableau_dashboard_spec.md`.
4. Save as `radiology_dashboard.twbx` inside this directory.
