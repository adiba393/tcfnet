# Multi-Granularity Network Traffic Forecasting

Time-series forecasting of mobile network KPIs (traffic volume, throughput, and
user count) at four spatial granularities — **Cell → Site → Thana → Area** —
using classical statistical models (ARIMA, VAR, Prophet) and deep-learning
models (LSTM, GRU, SVM/SVR, and a Transformer/CNN-LSTM hybrid).

This repository accompanies the thesis/paper *[add your paper title and link
here once available]*.

> **Note on this repository:** the code was originally developed as a
> collection of independent Jupyter notebooks written during iterative
> experimentation. This repo reorganizes those notebooks into a clearer
> structure for publication — see [Notes on this reorganization](#notes-on-this-reorganization)
> for exactly what changed.

## Repository structure

```
.
├── README.md
├── requirements.txt
├── .gitignore
├── data/
│   └── README.md              # data schema + where to place raw CSVs (not committed)
└── notebooks/
    ├── 00_pipeline_diagram/               # architecture/pipeline figure source
    ├── 01_data_exploration/               # initial data loading & sampling
    ├── 02_cell_level_forecasting/         # forecasts at the individual-cell level
    ├── 03_site_level_forecasting/         # forecasts at the site level
    ├── 04_thana_level_forecasting/        # forecasts at the thana (sub-district) level
    ├── 05_area_level_forecasting/         # forecasts at the area level
    ├── 06_cross_level_hierarchy_comparison/  # cell vs. site vs. thana, side by side
    ├── 07_correlation_and_multivariate_analysis/  # KPI correlation + multivariate models
    └── 08_model_comparison_and_evaluation/   # head-to-head model evaluation, train/test ratio study
```

The spatial hierarchy used throughout (from an Ericsson-style network export)
is: an **`EUTRANCELLFDD`** (cell) belongs to an **`ERBS`** (site/base station),
whose ID prefix identifies its **Thana** (a Bangladeshi sub-district-level
administrative unit), which in turn rolls up into an **Area**.

## Models and methods

| Category | Models used |
|---|---|
| Classical time-series | ARIMA, VAR (Vector AutoRegression), Prophet |
| Machine learning | SVM / SVR |
| Deep learning | LSTM, GRU, Bidirectional LSTM/GRU, CNN-LSTM hybrid, Transformer (attention-based) |
| Targets forecast | Total traffic volume, average throughput (TP), average number of users |

Common preprocessing across notebooks: `MinMaxScaler`/`StandardScaler`
normalization, lag-feature construction, and `train_test_split` /
chronological train-test splits. Evaluation uses MSE, MAE, and R².

## Notebook catalog

Every notebook below is standalone and can be opened/run independently,
provided the relevant CSV file (see [`data/README.md`](data/README.md)) is
placed in `data/` at the repo root.

### Pipeline / Architecture Diagram (`notebooks/00_pipeline_diagram/`)

| Notebook | Description | Original filename |
|---|---|---|
| `pipeline_diagram.ipynb` | Source for the overall pipeline/architecture figure (Mermaid + TikZ snippets). Not meant to be run top-to-bottom as Python -- open individual cells to extract the diagram source. | `Block Diagram.ipynb` |

### Data Exploration (`notebooks/01_data_exploration/`)

| Notebook | Description | Original filename |
|---|---|---|
| `area_prophet_and_cell_count_analysis.ipynb` | Exploratory notebook mixing an Area-level Prophet forecast (AVG_NO_USER) with ad-hoc analysis of how many cells exceed average traffic per day. Contains some duplicate/unused scratch cells. | `Thana wise forecast_cell count.ipynb` |
| `area_prophet_and_cell_count_analysis_v2.ipynb` | Alternate version of the notebook above. | `Thana wise forecast_cell count-Copy1.ipynb` |
| `data_sampling_exploration.ipynb` | Initial look at the raw KPI export: loads a CSV, inspects columns, and samples rows. | `Find Random data.ipynb` |
| `thana_prophet_exploratory.ipynb` | Early/exploratory Thana-level forecast using Prophet, grouping the raw data by date. | `Site wise forecast.ipynb` |

### Cell-Level Forecasting (`notebooks/02_cell_level_forecasting/`)

| Notebook | Description | Original filename |
|---|---|---|
| `arima_best_worst5_cells.ipynb` | ARIMA forecasts for the 5 best- and 5 worst-performing cells. | `Cell wise forecast based on ARIMA_F_best and worst 5 cells.ipynb` |
| `arima_best_worst5_cells_v2.ipynb` | Alternate run of the ARIMA best/worst-5-cells experiment. | `Cell wise forecast based on ARIMA_F_best and worst 5 cells-Copy1.ipynb` |
| `arima_normalized.ipynb` | ARIMA forecast at cell level with normalized inputs. | `Cell wise forecast based on ARIMA_Normalize.ipynb` |
| `lstm_model_comparison.ipynb` | Compares LSTM configurations at cell level. | `Cell wise forecast based on LSTM_Comparison.ipynb` |
| `tp_lstm_best_worst5_cells.ipynb` | LSTM throughput (TP) forecasts for the 5 best/worst cells. | `TP_Cell wise forecast based on LSTM_F_best and worst 5 cells.ipynb` |
| `tp_lstm_best_worst5_cells_v2.ipynb` | Alternate run of the notebook above. | `TP_Cell wise forecast based on LSTM_F_best and worst 5 cells-Copy1.ipynb` |
| `traffic_lstm_best_worst5_cells.ipynb` | LSTM traffic forecasts for the 5 best/worst cells. | `Traffic_Cell wise forecast based on LSTM_F_best and worst 5 cells.ipynb` |
| `traffic_lstm_oct.ipynb` | LSTM traffic forecast at cell level (October data). | `Cell wise forecast based on LSTM_Traffic_Oct.ipynb` |
| `user_lstm_gru_svm_normalized_adjusted.ipynb` | LSTM vs GRU vs SVM comparison for user-count forecasting at cell level, normalized data, timestamps realigned. | `Cell wise forecast based on LSTM_GRU_SVM_Final-User-Oct-Normalized-adjusting timestamp.ipynb` |

### Site-Level Forecasting (`notebooks/03_site_level_forecasting/`)

| Notebook | Description | Original filename |
|---|---|---|
| `tp_arima.ipynb` | ARIMA throughput forecast at site level. | `Site wise forecast based on ARIMA_TP.ipynb` |
| `tp_lstm.ipynb` | LSTM throughput forecast at site level. | `SIte wise forecast based on LSTM_TP.ipynb` |
| `tp_lstm_final_normalized_site1.ipynb` | Final normalized LSTM throughput forecast, Site 1 (`Site1data.csv`). | `Site wise forecast based on LSTM_Final-TP-Oct-Normalized.ipynb` |
| `tp_lstm_final_normalized_site2.ipynb` | Final normalized LSTM throughput forecast, Site 2 (`Site2data.csv`). | `Site wise forecast based on LSTM_Final-TP-Oct-Normalized-Site2.ipynb` |
| `tp_lstm_final_normalized_site3.ipynb` | Final normalized LSTM throughput forecast, Site 3 (`Site3data.csv`). | `Site wise forecast based on LSTM_Final-TP-Oct-Normalized-Site3.ipynb` |
| `traffic_lstm.ipynb` | LSTM traffic forecast at site level. | `SIte wise forecast based on LSTM_Traffic.ipynb` |
| `traffic_lstm_final_normalized_site1.ipynb` | Final normalized LSTM traffic forecast, Site 1. | `Site wise forecast based on LSTM_Final-Traffic-Oct-Normalized.ipynb` |
| `traffic_lstm_final_normalized_site2.ipynb` | Final normalized LSTM traffic forecast, Site 2. | `Site wise forecast based on LSTM_Final-Traffic-Oct-Normalized-Site2.ipynb` |
| `traffic_lstm_final_normalized_site3.ipynb` | Final normalized LSTM traffic forecast, Site 3. | `Site wise forecast based on LSTM_Final-Traffic-Oct-Normalized-Site3.ipynb` |
| `traffic_lstm_oct.ipynb` | LSTM traffic forecast at site level (October data). | `SIte wise forecast based on LSTM_Traffic_Oct.ipynb` |
| `traffic_user_arima_normalized.ipynb` | ARIMA traffic & user-count forecast at site level, normalized inputs. | `Site wise forecast based on ARIMA_Traffic&User_Normalized.ipynb` |
| `user_lstm_final_normalized_site1.ipynb` | Final normalized LSTM user-count forecast, Site 1. | `Site wise forecast based on LSTM_Final-User-Oct-Normalized.ipynb` |
| `user_lstm_final_normalized_site2.ipynb` | Final normalized LSTM user-count forecast, Site 2. | `Site wise forecast based on LSTM_Final-User-Oct-Normalized-Site2.ipynb` |
| `user_lstm_final_normalized_site3.ipynb` | Final normalized LSTM user-count forecast, Site 3. | `Site wise forecast based on LSTM_Final-User-Oct-Normalized-Site3.ipynb` |
| `user_lstm_gru_svm_normalized_adjusted_multisite.ipynb` | LSTM vs GRU vs SVM comparison for user-count forecasting across sites 1-4, normalized, timestamps realigned. | `Site wise forecast based on LSTM_GRU_SVM_Final-User-Oct-Normalized-adjusting timestamp.ipynb` |
| `user_lstm_v2.ipynb` | LSTM user-count forecast at site level (alternate run). | `SIte wise forecast based on LSTM_User-Copy1.ipynb` |

### Thana-Level Forecasting (`notebooks/04_thana_level_forecasting/`)

| Notebook | Description | Original filename |
|---|---|---|
| `arima_var.ipynb` | ARIMA and Vector AutoRegression (VAR) forecasts at Thana level. | `Thana wise forecast based on ARIMA&VAR.ipynb` |
| `lstm_baseline.ipynb` | Baseline LSTM forecast at Thana level. | `Thana wise forecast based on LSTM.ipynb` |
| `lstm_validation.ipynb` | Validation experiments for the Thana-level LSTM model. | `Thana wise forecast based on LSTM Validation.ipynb` |
| `tp_lstm_final.ipynb` | Final LSTM throughput forecast at Thana level. | `TP_Thana wise forecast based on LSTM_Final.ipynb` |
| `tp_lstm_final_prediction.ipynb` | Final LSTM throughput prediction at Thana level (separate run from `tp_lstm_final.ipynb`). | `Thana wise forecast based on LSTM_Final-TP prediction.ipynb` |
| `traffic_lstm_final_oct.ipynb` | Final LSTM traffic forecast at Thana level, October data. | `Thana wise forecast based on LSTM_Final-Traffic-Oct.ipynb` |
| `traffic_lstm_final_oct_alt.ipynb` | Separate experiment series for the same task (originally prefixed `Traffic_`), October data. | `Traffic_Thana wise forecast based on LSTM_Final-Oct.ipynb` |
| `traffic_lstm_final_oct_normalized.ipynb` | Final LSTM traffic forecast at Thana level, October data, normalized. | `Thana wise forecast based on LSTM_Final-Traffic-Oct-Normalized.ipynb` |
| `traffic_lstm_final_oct_v2.ipynb` | Final LSTM traffic forecast at Thana level, October data (alternate run). | `Thana wise forecast based on LSTM_Final-Traffic-Oct-Copy1.ipynb` |
| `user_lstm_final_normalized.ipynb` | Final normalized LSTM user-count forecast at Thana level. | `Thana wise forecast based on LSTM_Final-User-Oct-Normalized.ipynb` |
| `user_lstm_final_normalized_adjusted.ipynb` | Final normalized LSTM user-count forecast, timestamps realigned. | `Thana wise forecast based on LSTM_Final-User-Oct-Normalized-adjusting timestamp.ipynb` |
| `user_lstm_final_normalized_adjusted_v3.ipynb` | Final normalized LSTM user-count forecast, timestamps realigned (3rd iteration). | `Thana wise forecast based on LSTM_Final-User-Oct-Normalized-adjusting timestamp-Copy2.ipynb` |
| `user_lstm_gru_normalized_adjusted_v2.ipynb` | LSTM vs GRU comparison for user-count forecasting, normalized, timestamps realigned (alternate run). | `Thana wise forecast based on LSTM_GRU_Final-User-Oct-Normalized-adjusting timestamp--Copy1.ipynb` |
| `user_lstm_gru_svm_normalized_adjusted.ipynb` | LSTM vs GRU vs SVM comparison for user-count forecasting, normalized, timestamps realigned. | `Thana wise forecast based on LSTM_GRU_SVM_Final-User-Oct-Normalized-adjusting timestamp-.ipynb` |
| `user_lstm_gru_svm_normalized_adjusted_v2.ipynb` | Alternate run of the LSTM/GRU/SVM comparison above. | `Thana wise forecast based on LSTM_GRU_SVM_Final-User-Oct-Normalized-adjusting timestamp--Copy1.ipynb` |

### Area-Level Forecasting (`notebooks/05_area_level_forecasting/`)

| Notebook | Description | Original filename |
|---|---|---|
| `user_lstm_gru_svm_normalized_adjusted.ipynb` | LSTM vs GRU vs SVM comparison for user-count forecasting at Area level, normalized, timestamps realigned. | `Area wise forecast based on LSTM_GRU_SVM_Final-User-Oct-Normalized-adjusting timestamp.ipynb` |
| `user_transformer_lstm_gru_svm_normalized_adjusted.ipynb` | Adds a Transformer-based model to the LSTM/GRU/SVM comparison at Area level (PyTorch). | `Area wise forecast based on Transformer_LSTM_GRU_SVM_Final-User-Oct-Normalized-adjusting timestamp-Copy1.ipynb` |

### Cross-Level Hierarchy Comparison (`notebooks/06_cross_level_hierarchy_comparison/`)

| Notebook | Description | Original filename |
|---|---|---|
| `cell_site_thana_lstm.ipynb` | Builds Cell/Site/Thana grouped datasets from the raw export and runs LSTM forecasts across all three granularities for direct comparison. | `Cell Site Thana LSTM_F.ipynb` |
| `cell_site_thana_lstm_v2.ipynb` | Alternate run of the notebook above (PyTorch version). | `Cell Site Thana LSTM_F-Copy1.ipynb` |

### Correlation & Multivariate Analysis (`notebooks/07_correlation_and_multivariate_analysis/`)

| Notebook | Description | Original filename |
|---|---|---|
| `model_comparison_with_correlation_matrix.ipynb` | Compares LSTM/GRU/SVM alongside a feature-correlation matrix to motivate multivariate model inputs. | `Multivariant comparison with Correlation matrix.ipynb` |
| `multivariate_correlation_analysis.ipynb` | Correlation analysis between KPIs feeding the multivariate forecasting models. (A byte-identical duplicate of this notebook, `Multivariant with correlation.ipynb`, was removed during cleanup.) | `Multivariant for correlation.ipynb` |
| `thana_gru_multivariate.ipynb` | Multivariate GRU forecast at Thana level. | `Thana wise forecast based on Multivariant_GRU.ipynb` |

### Model Comparison & Evaluation (`notebooks/08_model_comparison_and_evaluation/`)

| Notebook | Description | Original filename |
|---|---|---|
| `area_lstm_gru_svm_evaluation.ipynb` | Extended evaluation (MSE/MAE/R2) of LSTM vs GRU vs SVM at Area level, largest/most thorough comparison notebook in the repo. | `Diff Evaluation Area wise forecast based on LSTM_GRU_SVM_Final-User-Oct-Normalized.ipynb` |
| `train_test_split_ratio_experiment.ipynb` | Studies the effect of different train/test split ratios on model performance. | `Train ratio experiment.ipynb` |

## Setup

```bash
git clone <this-repo-url>
cd <this-repo>

python3 -m venv .venv
source .venv/bin/activate      # Windows: .venv\Scripts\activate

pip install -r requirements.txt
```

`tensorflow` and `torch` are the two heaviest dependencies (deep-learning
models) — if you only need to run the classical-model notebooks (ARIMA/VAR/
Prophet, in `01_data_exploration`, `04_thana_level_forecasting/arima_var.ipynb`,
etc.) you can skip them.

## Data

Raw data is **not included** in this repository. See
[`data/README.md`](data/README.md) for the expected files and schema, and for
options on how to make the data available for reproducibility.

Once the CSV files are placed in `data/`, every notebook works out of the box —
data is loaded via relative paths (`../../data/<file>.csv`), so no path edits
are needed.

## How to run

Each notebook is self-contained: open it in Jupyter/JupyterLab and run all
cells top to bottom.

```bash
jupyter lab
```

There is no single end-to-end pipeline script — each notebook corresponds to
one experiment (a granularity × metric × model combination). Start from
`notebooks/01_data_exploration/` to see the raw data, then look at
`notebooks/08_model_comparison_and_evaluation/` for the head-to-head model
comparisons referenced in the paper.

## Notes on this reorganization

The notebooks were originally a flat collection of 55 files with ad-hoc names
(e.g. `Thana wise forecast based on LSTM_GRU_SVM_Final-User-Oct-Normalized-adjusting timestamp--Copy1.ipynb`).
To prepare this repo for publication, the following changes were made —
**no modeling logic, cell outputs, or results were altered**:

1. **Grouped into folders** by spatial granularity (cell/site/thana/area) and
   by purpose (data exploration, cross-level comparison, correlation analysis,
   model evaluation).
2. **Renamed** to short, consistent, descriptive filenames. The original
   filename for every notebook is kept in the [catalog table](#notebook-catalog)
   above for traceability back to the thesis/original work.
3. **Removed one exact duplicate** — `Multivariant with correlation.ipynb` was
   byte-for-byte identical to `Multivariant for correlation.ipynb`
   (now `07_correlation_and_multivariate_analysis/multivariate_correlation_analysis.ipynb`)
   and was dropped.
4. **Fixed hardcoded local file paths.** Several notebooks loaded data from an
   absolute path on the original author's machine
   (e.g. `C:\Users\HP\Desktop\IftiPython\HKUXC\...`). These were rewritten to
   the portable relative path `../../data/<filename>` so the notebooks run on
   any machine once the data is placed in `data/`.
5. **Added** this README, `data/README.md`, `requirements.txt`, and
   `.gitignore`.

A few notebooks kept for completeness are genuinely exploratory/scratch work
(noted individually in the catalog above) — e.g.
`01_data_exploration/area_prophet_and_cell_count_analysis.ipynb` contains some
duplicate/dead code cells from iterative development, and
`00_pipeline_diagram/pipeline_diagram.ipynb` holds diagram source (Mermaid +
TikZ) rather than runnable analysis code. These were left as-is rather than
rewritten, to avoid changing any experimental content.

## Citation

If you use this code, please cite the paper:

```bibtex
@article{your_citation_key,
  title   = {Your paper title},
  author  = {Your name},
  journal = {Journal / Conference},
  year    = {2026}
}
```

## License

No license file is included yet. Add one (e.g. MIT, Apache-2.0) before
making the repository public, so others know how they may reuse this code.
