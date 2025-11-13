## Overview

This repository contains two Jupyter notebooks forming a two-stage spatial analysis pipeline:

- **Stage 1 – `MAIN_script`**: Prepares and processes single-cell tabular data for downstream spatial analysis. This script performs the complete core workflow, including phenotyping, clustering, cell neighbourhood analysis, and intercellular distance calculations.  
- **Stage 2 – `Squidpy_script`** *(optional)*: Uses Squidpy for additional spatial analyses, such as creating Delaunay graphs, generating spatial plots suitable for later overlay on tissue images, and serving as a starting point for more advanced spatial statistics.

The pipeline was originally developed for a glioblastoma (GBM) spatial proteomics project but can be applied to any dataset of segmented single cells derived from multiplexed images of FFPE tumour sections from surgical tissue samples.

### Expected input data

The workflow expects a tabular dataset where:  
1. **Biomarker intensity values** are provided for each detected cell, calculated for the whole cell and/or subcellular compartments (e.g., nucleus, cytoplasm, membrane).  
2. **Spatial coordinates** (`X_centroid`, `Y_centroid`) represent the geometric centroid of each cell.  
3. *(Optional)* **Morphological measurements** (e.g., cell area, nucleus area) are available for quality control and filtering.

### Purpose

The pipeline enables the generation of:  
- Single-cell phenotyping maps  
- Phenotypic metrics (based on biomarker intensity patterns)  
- Spatial metrics, including cell neighbourhood composition and intercellular distances  
- Clustering visualisations (e.g., heatmaps, t-SNE, UMAP)  
- Optional Squidpy-based outputs for Delaunay graph analysis and advanced spatial inference

It can be adapted to match the research aims, including using different marker panels, thresholds, or clustering methods.

For full details of the cell segmentation workflow, see the accompanying [CEll_SEGMENTATION.md](CELL_SEGMENTATION.md) file.

---

**Note:** If you use or adapt these scripts, please acknowledge this repository in your work.
