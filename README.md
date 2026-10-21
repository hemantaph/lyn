# lyn — Wetland carbon and environmental visualisation

Seaborn/Jupyter analyses of soil, water and carbon measurements from **Loktak and Pumlen lakes**, covering **2022–2023**. All active notebooks are in [`ipynb_files_1/`](ipynb_files_1/).

## Notebooks

1. [Carbon seasons and scatterplots](ipynb_files_1/01_carbon_seasons_and_scatter.ipynb): source checks, seasonal carbon profiles, environmental associations and the corrected TBC scatterplots.
2. [Correlation heatmaps](ipynb_files_1/02_correlation_heatmaps.ipynb): supplied Pearson matrices, carbon-response overviews and checks against available measurements.
3. [Environment and PCA](ipynb_files_1/03_environment_and_pca.ipynb): environmental distributions; separate soil and water PCAs, both pooled and within each lake; combined-year biplots, loading heatmaps, scree plots, cumulative variance and thesis-style summary tables.

## Layout and inputs

```text
lyn/
├── ecological_data.py
├── ipynb_files_1/
│   ├── 01_carbon_seasons_and_scatter.ipynb
│   ├── 02_correlation_heatmaps.ipynb
│   ├── 03_environment_and_pca.ipynb
│   ├── figures/          # PNG and vector PDF exports
│   └── tables/           # CSV results and source-cell audits
└── README.md
```

Place these workbooks beside `ecological_data.py`:

| Workbook | Worksheets |
|---|---|
| `Scatter Plot (1).xlsx` | `ScatterPlot1`, `ScatterPlot2`, `ScatterPlot3`, `Line_Graph` |
| `PCA.xlsx` | `Water`, `Soil` |
| `Heatmap.xlsx` | `heatmap1`–`heatmap4` |

The thesis context documents are `Chapter 1-Introduction.docx` and `Chapter 2- Review of literature.docx`; they are not needed at runtime. **XLSX and DOCX files are ignored by Git** and must be supplied locally. Workbooks are read only. The parser depends on their supplied sheet layouts and explicit cell coordinates.

## Running the notebooks

Dependencies: Python, NumPy, pandas, openpyxl, Matplotlib, Seaborn, matplotlib-inline, Jupyter and ipykernel. The project uses Python 3.10.18 in the `ler` environment. PCA uses NumPy SVD rather than scikit-learn.

For a fresh environment:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install numpy pandas openpyxl 'matplotlib>=3.8' 'seaborn>=0.13' matplotlib-inline jupyterlab ipykernel
python -m ipykernel install --user --name lyn --display-name 'Python (lyn)'
jupyter lab
```

Open a notebook, select the installed kernel (or **Python (ler)**), and choose **Restart Kernel and Run All Cells**. Each notebook runs independently from its directory or the project root. Outputs are saved inside `ipynb_files_1/`. Rerunning overwrites the corresponding figures and tables.

## Figures and PCA conventions

Plots have no titles and use Times New Roman, 12-pt text, colour-blind-friendly lake colours and separate figures. Install Times New Roman locally to reproduce the font. Carbon seasonal figures are 140 × 93.3 mm; carbon scatterplots are 120 × 90 mm. Heatmaps and PCA biplots use larger canvases where needed; insert exports at their native sizes to retain font readability. Use landscape A4 when the figure width exceeds the thesis text width. Filenames and notebook markdown identify each analysis.

There are **six PCA fits**: pooled soil, pooled water, and soil/water separately for each lake. Each fit centres its variables and divides by sample SD within that fit. All PCA plots combine both years; there are no year-specific exports. Standalone score plots and correlation circles have been replaced by combined biplots. Scree plots, loading heatmaps and summary tables remain separate because they provide different information.

Biplots use explicitly documented reciprocal scaling of standardised PC scores and correlation loadings, with separate score and correlation axes. The circle is a correlation reference, not a confidence ellipse. Five well-represented variable arrows are displayed; all measured variables remain in the PCA. Full loadings, weights, eigenvalues, scores and standardisation parameters are exported. PC1–PC4 summary tables are a reporting format, not a component-retention rule. Independently fitted lake PC axes are not interchangeable; use the pooled PCA to compare lakes in a common coordinate system.

## Scientific limitations and checks

- Records are matched by **wetland, year, season and sampling site**, not row order. Repeated site observations are retained; they are not independent replicates for significance testing.
- `Scatter Plot (1).xlsx` is the corrected input: it replaces the 80 TBC values that previously duplicated TDS. TBC scatterplots and seasonal means/SD now use these corrected observations. The unchanged `Line_Graph` summaries are retained for comparison, and the correlation audit includes TBC. The original workbook is not overwritten.
- Seasonal error bars calculated from available raw carbon measurements are sample SD across five sites. Source-summary discrepancies are retained in the audit tables.
- Measurement headers omit some units. Plot units follow the project's display conventions and should be confirmed against the measurement protocol.
- Supplied correlation coefficients and recomputed coefficients are retained separately. No new significance tests, causal claims or sequestration-rate estimates are introduced.
- PCA checks verify reconstruction of the standardised data, the eigenvalue sum and equality of correlation loadings with directly computed correlations. Display-only log-axis choices do not transform PCA or correlation inputs.

The notebooks include context, equations, source limitations and interpretation guidance alongside the code.
