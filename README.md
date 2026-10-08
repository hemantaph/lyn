# lyn — Wetland carbon and environmental data visualisation

Jupyter notebooks for exploring seasonal carbon measurements and water/soil conditions in **Loktak and Pumlen lakes, Manipur**, using observations from **2022–2023**. The analysis uses Seaborn for plots and NumPy for principal component analysis (PCA).

The notebooks provide descriptive comparisons and associations. They do not estimate sequestration rates, establish causation, or treat repeated observations as independent sites.

## Notebooks

Three independently runnable notebooks use the local Excel data and the two thesis chapters as context:

1. **[01_carbon_seasons_and_scatter.ipynb](ipynb_files_2/01_carbon_seasons_and_scatter.ipynb)** — source checks, seasonal carbon profiles, soil-stock/environment and phytoplankton/environment scatter plots, and pooled versus within-lake correlations.
2. **[02_correlation_heatmaps.ipynb](ipynb_files_2/02_correlation_heatmaps.ipynb)** — all four supplied correlation matrices, a carbon-response overview, and comparison with the available underlying measurements.
3. **[03_environment_and_pca.ipynb](ipynb_files_2/03_environment_and_pca.ipynb)** — distributions of every water/soil variable, separate standardised PCAs via NumPy SVD, scree plots, scores by year and separate correlation-loading figures.

Start with these three notebooks in [`ipynb_files_2/`](ipynb_files_2/). [`ipynb_files_1/`](ipynb_files_1/) contains alternative compact-layout versions. Each collection saves its own figures and numerical tables inside its notebook directory, so running one collection does not overwrite the other’s outputs.

## Project layout

```text
lyn/
├── ecological_data.py     # Workbook parsing and source-cell coordinates
├── ipynb_files_1/          # Alternative compact-layout notebooks
│   ├── *.ipynb
│   ├── figures/           # Figure exports for this collection
│   └── tables/            # CSV outputs for this collection
├── ipynb_files_2/          # A4 analysis notebooks
│   ├── 01_carbon_seasons_and_scatter.ipynb
│   ├── 02_correlation_heatmaps.ipynb
│   ├── 03_environment_and_pca.ipynb
│   ├── figures/           # Recommended thesis figures and A4 proof
│   └── tables/            # Measurements, results and source audits
└── README.md
```

Saved notebook outputs, figure exports and CSV tables are included. **Source `.xlsx` workbooks and `.docx` documents are excluded by `.gitignore`** and must be supplied locally to reproduce the analysis.

## Local input files

Place the following workbooks in the repository root, beside `ecological_data.py`, with these exact filenames:

| File | Expected worksheets | Purpose |
| --- | --- | --- |
| `PCA.xlsx` | `Water`, `Soil` | Environmental measurements and sampling identifiers |
| `Scatter Plot.xlsx` | `ScatterPlot1`, `ScatterPlot2`, `ScatterPlot3`, `Line_Graph` | Carbon measurements, environmental blocks and seasonal summaries |
| `Heatmap.xlsx` | `heatmap1`–`heatmap4` | Reported Pearson coefficients, p-values and sample sizes |

The context documents are `Chapter 1-Introduction.docx` and `Chapter 2- Review of literature.docx`. These are not required at runtime.

The parser expects the supplied workbook layout, including explicit cell coordinates for the hand-formatted sheets. If a workbook is reorganised, update the corresponding coordinates in `ecological_data.py`. Do not pair observations by row position: their keys are wetland, year, season and site.

## Setup and execution

The notebooks have been run with **Python 3.10.18**, pandas 2.3.0, Seaborn 0.13.2 and Matplotlib 3.10.3. Dependencies are NumPy, pandas, openpyxl, Matplotlib, Seaborn and Jupyter. PCA uses NumPy SVD and does not require scikit-learn.

```bash
git clone git@github.com:hemantaph/lyn.git
cd lyn

python3 -m venv .venv
source .venv/bin/activate
python -m pip install numpy pandas openpyxl "matplotlib>=3.8" "seaborn>=0.13" jupyterlab ipykernel
python -m ipykernel install --user --name lyn --display-name "Python (lyn)"
jupyter lab
```

After adding the local workbooks, open a notebook in `ipynb_files_2/`, select **Python (lyn)**, and choose **Restart Kernel and Run All Cells**. Existing users of the `ler` environment can select **Python (ler)** instead. All six notebooks locate the helper module from either the project root or their notebook directory. Input workbooks stay in the project root; generated figures and tables go into the corresponding notebook directory.

Run the cells in order: the loading cell builds the keyed data used by the plots. Shared variables are checked before merging, and duplicate measurements are not added as separate observations. Source files are read only. Rerunning a notebook overwrites its generated figures and tables.

## Figures for an A4 thesis

The revised figures use Seaborn, colour-blind-friendly lake colours and blue–orange correlation maps. **Use the A4 exports in `ipynb_files_2/figures/`**. The compact-layout exports are in `ipynb_files_1/figures/`.

Each current figure is **160 mm wide**, with 9.5–12 pt labels at that width, and is saved as a **300-dpi PNG and vector PDF**. This fits an A4 page with 25-mm side margins. Insert the individual PDFs at full text width; reducing them to half-width will also shrink the labels. [A4_figure_proof.pdf](ipynb_files_2/figures/A4_figure_proof.pdf) shows all 43 figures at their native size on A4 pages, with explanatory captions. It is a layout proof and complete figure collection, not a recommendation to include all 43 figures in the thesis.

Year and variable panels are stacked vertically. Scatter plots use one predictor per figure and separate lake panels; their shared x-ranges and lake-specific y-ranges are explicitly labelled. Water correlation matrices are split into three blocks that together retain the entire lower triangle. PCA scores and loadings have separate figures to avoid mixing their coordinates. Distribution plots identify the two years using point shape.

Numerical results and source audits are under each collection’s `tables/` directory. The setup cell's `SCALE_OVERRIDES` allows display-only log axes. The current figures use linear scales: grouping and layout resolve the crowding without transforming the observations. Neither correlations nor PCA uses log-transformed inputs.

Every notebook includes study context, a contents guide, explanations beside the analysis, and conclusions tied to the supplied measurements. These conclusions apply to the current workbooks and should be revisited if the data change.

## Source issues retained explicitly

- **Raw TBC is unavailable:** all 80 values in `ScatterPlot2!AK6:AN10`, `AP6:AS10`, `AK14:AN18`, and `AP14:AS18`, headed “Total Biomass Carbon”, exactly match TDS in `PCA.xlsx`. These values are not used as biomass. Seasonal TBC figures use only `Line_Graph!F8:I11`; TBC heatmaps show the supplied coefficients and are labelled as reported results. Corrected site-level TBC is needed for valid TBC scatter plots.
- **One phytoplankton mean differs:** Loktak, 2022, monsoon has a site-data mean of **493.898**, while `Line_Graph!G13` reports **489.89**. Site-level plots use the recomputed mean. Other mean/spread differences are recorded in `ipynb_files_2/tables/seasonal_source_audit.csv`.
- **Units and spread definitions:** environmental and phytoplankton-carbon units are not supplied in the measurement headers. No guessed units are added in the A4 notebooks. The TBC summary's unit and the meaning of “±” in `Line_Graph` are unconfirmed. Raw-data seasonal error bars are explicitly computed sample SD across five sites. The A4 TBC plot shows means only; the original “±” values are retained in `ipynb_files_2/tables/seasonal_summary_as_reported.csv` until their meaning is confirmed.
- **Correlation tables differ slightly from the supplied observations:** maximum absolute differences in Pearson r are 0.00854 for the soil matrices and 0.00348 for the water matrices. Both reported and recomputed values are retained in `ipynb_files_2/tables/correlation_source_audit.csv`; the cause is not established.
- **Identifier mapping is verified:** matching 80 records for each of nine overlapping variables establishes wetland 1 = Loktak, 2 = Pumlen; year 1 = 2022, 2 = 2023; season 1–4 = pre-monsoon, monsoon, post-monsoon, winter.

All source workbooks remain unchanged. The plots are descriptive: repeated sites are retained, and no new significance tests, adjusted regressions or causal claims are introduced.

## Scientific and rendering checks

During the A4 revision, all three A4 notebooks executed in `ler` without errors or warnings. The 32 pre-existing numerical tables were checked against their earlier versions and were unchanged by that revision. PCA eigenvalues were independently checked by eigendecomposition of each compartment's correlation matrix, in addition to the reconstruction and correlation-loading checks in the notebook. All 43 A4 proof pages were rendered and visually inspected for layout, captions and legibility.

After relocating the outputs, all six notebooks were executed from their own directories in `ler`, with no errors or warnings. Setup cells were also checked from the project root. The 32 shared audit/result tables matched the previous outputs. The carbon measurement exports retain each collection’s existing column selection, with identical values in shared columns. Only output paths and directory documentation changed in the notebook source during this move. Conclusions and source audits describe the supplied workbooks and should be revisited if the data change.
