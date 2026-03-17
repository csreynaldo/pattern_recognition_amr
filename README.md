# AMR Thesis Project: Antimicrobial Resistance Pattern Recognition

An analytical pipeline for antimicrobial resistance (AMR) pattern recognition and surveillance, designed for the analysis of bacterial isolates from environmental and clinical samples.

## Project Overview

This project implements a comprehensive data science pipeline for AMR surveillance, including:

- **Phase 1**: Research design and data collection (INOHAC AMR Project Two — laboratory/field work)
- **Phase 2**: Data preprocessing (ingestion, cleaning, encoding, feature engineering)
- **Phase 3**: Unsupervised structure identification (hierarchical clustering, visualization)
- **Phase 4**: Supervised learning for pattern discrimination
- **Phase 5**: Regional and environmental analysis (PCA, distribution analysis)
- **Phase 6**: Integration and synthesis
- **Phase 7**: Interactive Streamlit dashboard
- **Phase 8**: Documentation (see [docs/TECHNICAL_REFERENCE.md](docs/TECHNICAL_REFERENCE.md))

> **📖 For step-by-step usage instructions, see [USER_MANUAL.md](USER_MANUAL.md)**
>
> **🔬 For technical parameters and results summary, see [docs/TECHNICAL_REFERENCE.md](docs/TECHNICAL_REFERENCE.md)**
>
> **⚠️ For study limitations and scope, see [docs/limitations.md](docs/limitations.md)**

### Documentation Structure

```
docs/
├── TECHNICAL_REFERENCE.md      # Condensed technical summary
├── limitations.md              # Study limitations
└── archive/                    # Detailed reference notes
    ├── methods/                # Technical method notes
    └── results/                # Phase results documentation
```

## Isolate Code Convention

The isolate codes follow this naming convention:

- **National Site**: O (Ormoc), P (Pampanga), M (Marawi/BARMM)
- **Local Site**: A (Alegria), L (Larrazabal), G (Gabriel), R (Roque), D (Dayawan), T (Tuca Kialdan)
- **Sample Source**: DW (Drinking Water), LW (Lake Water), FB (Fish Banak), FG (Fish Gusaw), RW (River Water), FT (Fish Tilapia), EWU (Effluent Water Untreated), EWT (Effluent Water Treated), FK (Fish Kaolang)
- **Replicate**: 1, 2, 3
- **Colony**: 1, 2, 3, etc.

Example: `EC_OADWR1C3` = Escherichia coli from Ormoc, Alegria site, Drinking Water, Replicate 1, Colony 3

## Installation

1. Clone the repository:

```bash
git clone https://github.com/your-repo/amr-thesis-project.git
cd amr-thesis-project
```

2. Install dependencies:

```bash
pip install -r requirements.txt
```

## Usage

### Central CLI (main.py)

**All commands go through `main.py`** — never run scripts directly.

```bash
# Activate virtual environment first
.\venv\Scripts\Activate.ps1     # Windows
source venv/bin/activate         # Linux/Mac

# Available commands
python main.py --pipeline     # Core data flow: Ingestion → Cleaning → Encoding → Clustering
python main.py --validate     # Run validation scripts (cluster validation, co-resistance, antibiotic clustering)
python main.py --sensitivity  # Run sensitivity analysis (encoding schemes, data thresholds)
python main.py --analyze      # Run analysis modules (regional, integration, supervised)
python main.py --viz          # Regenerate all visualizations
python main.py --app          # Launch Streamlit dashboard
python main.py --all          # Run everything in sequence (pipeline → validate → sensitivity → analyze → viz)

# Optional: Override cluster count (data-driven k selection by default)
python main.py --pipeline --k 4   # Force k=4 clusters (overrides auto-detection)
```

### Interactive Dashboard

```bash
python main.py --app
```

The dashboard opens at `http://localhost:8501` and provides:

- Data overview and exploration (isolate counts, species distribution)
- Resistance heatmaps (antibiotic × cluster)
- Cluster analysis (dynamically selected k with justification)
- Antibiotic clustering (co-resistance patterns)
- PCA visualization (by cluster, region, environment, MDR status)
- Regional & environmental distribution analysis
- Model evaluation summaries (species classifier, co-resistance predictors)

## Project Structure

```
amr_thesis_project_code/
├── main.py                       # 🔴 CENTRAL ENTRY POINT (CLI)
├── README.md                     # This file
├── requirements.txt              # Dependencies
├── app/
│   └── streamlit_app.py          # Interactive dashboard
├── data/
│   ├── raw/                      # Raw CSV data files (place input files here)
│   └── processed/                # Pipeline outputs
│       ├── figures/              # All visualizations (39+ files)
│       └── clustering_artifacts/ # Reproducibility artifacts (linkage matrix, etc.)
├── models/
│   └── species_classifier.joblib # Trained species classifier
├── src/
│   ├── config.py                 # 🔧 CENTRAL CONFIGURATION (single source of truth)
│   ├── preprocessing/            # Data processing modules
│   │   ├── data_ingestion.py     # CSV consolidation & metadata extraction
│   │   ├── data_cleaning.py      # Validation & standardization
│   │   ├── resistance_encoding.py # S/I/R → numeric encoding
│   │   └── feature_engineering.py # MDR, MAR index computation
│   ├── clustering/               # Hierarchical clustering
│   ├── supervised/               # ML models (species discrimination)
│   ├── analysis/                 # Regional/environmental analysis
│   ├── visualization/            # Plot generation
│   └── utils/                    # Shared utilities (console styling)
├── scripts/                      # Standalone analysis scripts
│   ├── validate_clustering.py    # Silhouette + elbow analysis
│   ├── coresistance_analysis.py  # Co-resistance network
│   ├── antibiotic_clustering.py  # Antibiotic grouping by patterns
│   ├── encoding_sensitivity.py   # Encoding scheme comparison
│   └── threshold_sensitivity.py  # Data threshold analysis
└── docs/
    ├── USER_MANUAL.md            # Step-by-step usage guide
    ├── TECHNICAL_REFERENCE.md    # Condensed technical summary
    ├── limitations.md            # Study scope & limitations
    └── archive/                  # Detailed reference notes
```

## Data Encoding

### Resistance Values

- **S (Susceptible)** → 0
- **I (Intermediate)** → 1
- **R (Resistant)** → 2

### MDR Definition

Multi-Drug Resistant (MDR) isolates are defined as those resistant to ≥3 antibiotic classes.

### MAR Index

Multiple Antibiotic Resistance (MAR) Index = Number of antibiotics resistant / Total antibiotics tested

## Output Files

After running the pipeline, the following files are generated in `data/processed/`:

### Data Files

| File                         | Description                              |
| ---------------------------- | ---------------------------------------- |
| `unified_raw_dataset.csv`    | Consolidated raw data from all sources   |
| `cleaned_dataset.csv`        | Cleaned and standardized data            |
| `cleaning_report.txt`        | Documentation of cleaning decisions      |
| `encoded_dataset.csv`        | Numerically encoded resistance data      |
| `analysis_ready_dataset.csv` | Final dataset with MDR, MAR computed     |
| `clustered_dataset.csv`      | Dataset with cluster assignments         |
| `feature_matrix_X.csv`       | Pure feature matrix for clustering       |
| `metadata.csv`               | Isolate metadata (region, species, etc.) |

### Figures (`figures/`)

| Category              | Files                                                                                 |
| --------------------- | ------------------------------------------------------------------------------------- |
| Clustering            | `cluster_validation.png`, `dendrogram_highres.png`, `cluster_profiles.png`            |
| Antibiotic clustering | `antibiotic_dendrogram.png`, `antibiotic_clustered_heatmap.png`                       |
| Co-resistance         | `coresistance_network.png`, `coresistance_matrix.csv`                                 |
| PCA                   | `pca_by_cluster.png`, `pca_by_region.png`, `pca_by_environment.png`, `pca_biplot.png` |
| Distribution          | `mar_distribution.png`, `mdr_distribution.png`, `resistance_heatmap.png`              |

### Clustering Artifacts (`clustering_artifacts/`)

- `linkage_matrix.pkl`: Reproducible linkage matrix
- `clustering_info.pkl`: Parameters and justifications
- `robustness_analysis.pkl`: Manhattan distance robustness check

## Key Terminology

This project uses the following standardized terminology:

- **Pattern Discrimination**: Supervised learning to evaluate how resistance patterns distinguish known categories
- **Model Evaluation**: Quantifying model performance to assess pattern consistency
- **Structure Identification**: Unsupervised discovery of natural groupings in resistance data

For detailed terminology definitions, see [docs/TECHNICAL_REFERENCE.md](docs/TECHNICAL_REFERENCE.md).

## Quick Troubleshooting

| Issue                         | Quick Fix                                                                |
| ----------------------------- | ------------------------------------------------------------------------ |
| `ModuleNotFoundError`         | Activate virtual environment: `.\\venv\\Scripts\\Activate.ps1` (Windows) |
| Dashboard shows outdated data | Re-run pipeline: `python main.py --pipeline --validate --viz`            |
| `No data loaded` error        | Ensure CSV files are in `data/raw/` directory                            |
| Matplotlib threading errors   | Set backend: `$env:MPLBACKEND='Agg'; python main.py --all`               |
| Missing output files          | Run full pipeline: `python main.py --all`                                |

**For detailed troubleshooting**, see [USER_MANUAL.md § 9 Troubleshooting](USER_MANUAL.md#9-troubleshooting).

## Data Source

The antimicrobial susceptibility testing (AST) data used in this study was collected and provided by the **INOHAC AMR Project Two** research team. The dataset comprises bacterial isolates from the Water-Fish-Human nexus across three Philippine regions (Ormoc, Pampanga, Marawi/BARMM).

> **Acknowledgment**: This computational analysis would not be possible without the laboratory work and data collection efforts of the INOHAC AMR Project Two team.

## Disclaimer

⚠️ **This tool is intended for exploratory pattern recognition and surveillance analysis only.** It should not be used for clinical decision support. No patient-level identifiers are processed.

## Antibiotic Classes

The pipeline recognizes the following antibiotic classes for MDR calculation:

- Penicillins (AM, AMP)
- β-lactam/β-lactamase inhibitor combinations (AMC, PRA)
- Cephalosporins - 1st generation (CN, CF)
- Cephalosporins - 3rd/4th generation (CPD, CTX, CFT, CPT)
- Cephamycins (CFO)
- Carbapenems (IPM, MRB)
- Aminoglycosides (AN, GM, N)
- Quinolones/Fluoroquinolones (NAL, ENR)
- Tetracyclines (DO, TE)
- Nitrofurans (FT)
- Phenicols (C)
- Folate pathway inhibitors (SXT)

## License

This project is developed for academic research purposes as part of a thesis study.

## Authors

**Reynaldo Jr. Pahay and Al-Hanif Magomnang**
Thesis Project — Antimicrobial Resistance Pattern Recognition  
Data Source: INOHAC AMR Project Two
