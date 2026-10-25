# Spatial Transcriptomics of the Breast Cancer Tumor Microenvironment

An exploratory computational biology project analyzing spatial gene expression in breast cancer tissue using Python, Scanpy, and spatial nearest-neighbor analysis.

## Project Overview

The tumor microenvironment contains cancer cells, immune cells, and other cell populations whose spatial organization may help researchers understand tumor biology.

In this project, I analyze a public Xenium spatial transcriptomics dataset from 10x Genomics to explore the spatial distribution of marker-associated cell populations and their nearest-neighbor relationships.

The project demonstrates a reproducible workflow for data exploration, quality assessment, spatial visualization, and exploratory spatial analysis.

## Research Question

**How are epithelial-associated and T-cell-associated populations spatially distributed in breast cancer tissue, and how does their nearest-neighbor distance compare with a locally randomized spatial baseline?**

## Dataset

- **Dataset:** Xenium FFPE Human Breast Cancer Replicate 1
- **Source:** 10x Genomics public Xenium dataset
- **Data type:** Spatially resolved gene expression
- **Cells:** 167,780
- **Genes in the analyzed expression matrix:** 313

The analysis uses a targeted gene panel. Consequently, marker detection and cell-group definitions are limited to the genes measured in this dataset.

Please consult the original dataset documentation for the data-generation details, coordinate units, and usage terms.

## Tools and Technologies

- Python
- Google Colab
- Scanpy
- AnnData
- NumPy and pandas
- SciPy spatial data structures
- Matplotlib
- Git and GitHub

## Analysis Workflow

### 1. Data exploration

- Loaded the expression matrix and cell metadata.
- Checked cell identifiers and matched expression data with metadata.
- Verified the availability of spatial coordinates and selected marker genes.

### 2. Quality control

- Examined transcript counts and detected-gene distributions.
- Checked coordinate validity and marker availability.
- Generated summary tables and quality-control visualizations.

Quality-control findings were used to characterize the dataset rather than applying arbitrary filtering thresholds.

### 3. Spatial visualization

- Visualized the distribution of cells across the tissue.
- Plotted expression patterns for selected epithelial and immune-associated markers.
- Generated spatial marker panels and marker-detection summaries.

Markers examined included EPCAM, KRT8, PTPRC, CD3D, CD3E, CD68, MS4A1, ACTA2, and PECAM1.

### 4. Cell neighborhood analysis

For an exploratory comparison, I defined:

- **Epithelial-associated cells:** EPCAM > 0 and KRT8 > 0.
- **T-cell-associated cells:** CD3D > 0 and CD3E > 0.

Cells meeting both definitions were excluded from the exclusive-group nearest-neighbor comparison.

I then calculated the nearest epithelial-associated cell distance for each T-cell-associated cell and compared the observed median distance with a spatial label-permutation baseline using several grid resolutions.

These are marker-based operational definitions, not independently validated cell-type annotations.

## Preliminary Results

### Cell groups

| Measurement | Number of cells |
|---|---:|
| EPCAM-positive and KRT8-positive | 80,057 |
| CD3D-positive and CD3E-positive | 15,003 |
| Meeting both definitions | 1,944 |
| Epithelial-associated only | 78,113 |
| T-cell-associated only | 13,059 |

### Nearest-neighbor distances

The median nearest-neighbor distance from T-cell-associated cells to epithelial-associated cells was **33.14 dataset coordinate units**.

| Grid resolution | Observed median | Mean randomized median |
|---|---:|---:|
| 10 × 10 | 33.14 | 11.61 |
| 20 × 20 | 33.14 | 13.23 |
| 40 × 40 | 33.14 | 16.90 |

Across the three grid resolutions, the observed median exceeded all 100 randomized medians in each test.

This suggests separation relative to the tested randomization models. However, the randomized baseline changed with grid resolution, and the current model does not fully preserve tissue compartments or fine-scale spatial structure.

**Interpretation:** These findings are preliminary. They do not establish that T cells are biologically excluded from tumor regions, nor do they demonstrate direct cell-cell interactions.

## Repository Structure

The repository is organized into separate notebooks so each stage can be run and reviewed independently.

```text
spatial-transcriptomics-tumor-microenvironment/
├── README.md
├── notebooks/
│   ├── 01_data_exploration.ipynb
│   ├── 02_quality_control.ipynb
│   ├── 03_spatial_visualization.ipynb
│   └── 04_cell_neighborhoods.ipynb
├── results/
│   ├── figures/
│   └── tables/
└── requirements.txt
```

The structure above is the intended layout. Adjust folder names to match the files actually committed to the repository.

## Reproducibility

The analysis was developed in Google Colab using Python-based scientific computing tools. A fixed random seed was used for the spatial permutation analysis.

To reproduce the project:

1. Obtain the public dataset from 10x Genomics.
2. Follow the notebook instructions to prepare the input AnnData file.
3. Run the notebooks in numerical order.
4. Inspect the saved tables and figures.
5. Review the analysis manifests for settings and limitations.

The raw dataset is not included in this repository by default. Dataset download and preparation instructions should be completed and tested before claiming full end-to-end reproducibility.

## Limitations

- The expression matrix contains a targeted panel of 313 genes.
- Marker expression alone does not establish definitive cell identities.
- Strict marker co-expression may exclude cells with low or undetected marker expression.
- Spatial coordinate units should be verified before converting distances to physical units.
- The local permutation model depends on grid resolution and does not fully account for tissue architecture.
- Nearest-neighbor distance is a measure of spatial proximity, not proof of biological interaction or causation.
- The findings require additional validation and sensitivity analysis before stronger biological conclusions can be drawn.

## Future Directions

- Validate cell-group definitions using a more comprehensive marker strategy.
- Assess tissue compartments and spatial density before selecting a null model.
- Increase permutation counts and conduct additional sensitivity analyses.
- Explore local cell composition and neighborhood enrichment.
- Improve end-to-end reproducibility with a tested environment specification and clear dataset preparation instructions.

## About This Project

This is a self-directed learning and portfolio project focused on applying computational methods to spatial transcriptomics data. It demonstrates practical experience with single-cell expression data structures, marker-based analysis, spatial visualization, nearest-neighbor calculations, and reproducible scientific workflows.
