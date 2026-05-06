# AI Impact on Radiologist Efficiency

> KNIME workflows and Tableau dashboards analyzing AI-assisted diagnostic tool impact on radiologist speed and accuracy

## Overview

This project analyzes how AI influences radiologists' efficiency when interpreting chest X-rays. An NLP classification pipeline was built on MIMIC-CXR reports with sentiment analysis and a Power BI dashboard for real-time urgency triage — covering **11,200+ AI-assisted imaging records**.

**Key Result:** AI-assisted workflows improved turnaround efficiency. Complex cases: 34% | Routine AI-assisted: 29% | Review/QA: 23%.

## Tools & Technologies

| Tool | Purpose |
|------|---------|
| KNIME Analytics Platform | Workflow automation and NLP pipeline |
| Tableau | Interactive dashboards and visual analytics |
| Power BI (DAX) | Real-time urgency triage dashboard |
| MIMIC-CXR | Chest X-ray radiology reports dataset |
| NLP | Sentiment analysis on radiology reports |

## Repository Structure

```
ai-radiologist-efficiency/
├── README.md
├── knime_workflows/
│   └── radiologist_efficiency_pipeline.knwf   # KNIME workflow export
├── dashboards/
│   └── radiology_dashboard.twbx               # Tableau workbook
├── notebooks/
│   └── nlp_classification.ipynb               # NLP pipeline notebook
├── data/
│   └── sample_reports.csv                     # Sample radiology report data
├── config/
│   └── config.yaml
└── requirements.txt
```

## Environment Setup

### Prerequisites
- KNIME Analytics Platform 5.x ([Download](https://www.knime.com/downloads))
- Tableau Desktop or Tableau Public ([Download](https://public.tableau.com/))
- Python 3.9+ (for NLP notebook)
- MIMIC-CXR access ([PhysioNet credentials required](https://physionet.org/content/mimic-cxr/))

### Python Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/JNKAnalyst/ai-radiologist-efficiency.git
   cd ai-radiologist-efficiency
   ```

2. **Create a virtual environment**
   ```bash
   python -m venv venv
   source venv/bin/activate        # Mac/Linux
   venv\Scripts\activate           # Windows
   ```

3. **Install Python dependencies**
   ```bash
   pip install -r requirements.txt
   ```

### KNIME Setup

1. Open KNIME Analytics Platform
2. Go to `File → Import KNIME Workflow`
3. Select `knime_workflows/radiologist_efficiency_pipeline.knwf`
4. Update the CSV Reader node to point to your MIMIC-CXR data path
5. Run the workflow top-to-bottom

### Tableau Setup

1. Open `dashboards/radiology_dashboard.twbx` in Tableau Desktop or Tableau Public
2. If prompted, reconnect the data source to your local CSV output from the KNIME workflow

## Configuration

### `config/config.yaml`
```yaml
data:
  mimic_cxr_path: "data/mimic_cxr_reports.csv"
  output_path: "data/processed_output.csv"

nlp:
  model: "vader"                   # Sentiment analysis model
  urgency_threshold: 0.65          # Score above = urgent triage flag
  max_report_length: 512

knime:
  workflow_path: "knime_workflows/radiologist_efficiency_pipeline.knwf"
```

## Running the NLP Pipeline

```bash
# Run NLP classification notebook
jupyter notebook notebooks/nlp_classification.ipynb
```

## Requirements

```
pandas==2.1.0
numpy==1.26.0
nltk==3.8.1
vaderSentiment==3.3.2
scikit-learn==1.3.0
matplotlib==3.8.0
seaborn==0.13.0
jupyter==1.0.0
pyyaml==6.0
```

## Data Access

This project uses the **MIMIC-CXR** dataset. Access requires:
1. Complete CITI training
2. Sign the data use agreement on [PhysioNet](https://physionet.org/content/mimic-cxr/)
3. Place downloaded data at the path in `config.yaml`

## Author

**Joash**  
[GitHub](https://github.com/JNKAnalyst) | [Portfolio](https://jnkanalyst.github.io/portfolio/)
