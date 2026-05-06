# KNIME Pipeline Specification

End-to-end node layout for `radiologist_efficiency_pipeline.knwf`.

## Nodes (in order)

1. **CSV Reader** — reads `data/mimic_cxr_reports.csv` (path from `config/config.yaml::data.mimic_cxr_path`). Columns: `study_id`, `case_type`, `ai_assisted`, `turnaround_minutes`, `report_text`.
2. **Missing Value** — replaces nulls in `report_text` with empty string; drops rows with missing `study_id`.
3. **String Manipulation** — lowercases `report_text` and trims to 512 characters (matches `nlp.max_report_length`).
4. **Strings To Document** — converts `report_text` into a Document cell for KNIME Text Processing nodes.
5. **Punctuation Erasure → Stop Word Filter → Snowball Stemmer** — standard NLP preprocessing chain.
6. **Bag Of Words Creator** — produces term/document table.
7. **VADER Sentiment (Python Script node)** — calls `vaderSentiment` to score each document; appends `neg`, `neu`, `pos`, `compound`.
8. **Rule Engine** — derives `urgency_score = 0.6 * neg + 0.4 * keyword_signal` where `keyword_signal` flags presence of urgent terms (pneumothorax, tension, acute, hemorrhage, severe, rapidly, worsening, critical, emergent).
9. **Numeric Binner** — flags `urgent_flag = 1` when `urgency_score >= 0.65` (matches `nlp.urgency_threshold`).
10. **GroupBy** — aggregates by `case_type` and `ai_assisted` to produce mean turnaround minutes and percent urgent.
11. **CSV Writer** — writes `data/processed_output.csv` (path from `config/config.yaml::data.output_path`). This is the file consumed by the Tableau workbook.

## Notes

- The Python Script node in step 7 requires a KNIME Python environment with `vaderSentiment` installed. Use the same `requirements.txt` from the project root.
- Cycle time for the full pipeline on the 11,200-record MIMIC-CXR sample is approximately 4–6 minutes on a laptop-class machine.
