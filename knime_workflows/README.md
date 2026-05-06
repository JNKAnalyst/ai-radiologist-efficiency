# KNIME Workflows

This directory holds the KNIME Analytics Platform workflow used to ingest MIMIC-CXR radiology reports, run the NLP urgency classification, and export per-study results for downstream Tableau and Power BI dashboards.

## Files

- `radiologist_efficiency_pipeline.knwf` — KNIME workflow export (binary). Not committed to this repository because it depends on a local KNIME installation and on MIMIC-CXR data that requires a PhysioNet data use agreement. See `pipeline_spec.md` for a full description of the nodes and their configuration so the workflow can be reconstructed in KNIME 5.x.

## Reconstructing the workflow

1. Install KNIME Analytics Platform 5.x.
2. Follow the node-by-node specification in `pipeline_spec.md`.
3. Point the **CSV Reader** at your local MIMIC-CXR reports CSV (path defined in `config/config.yaml`).
4. Run the workflow top-to-bottom; the final **CSV Writer** produces the file consumed by the Tableau workbook in `dashboards/`.
