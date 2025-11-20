# Cell Segmentation Workflow

The QPTIFF files used in this project were processed using open-source image analysis software **QuPath (v0.6.0)**, which incorporates deep-learning models to improve segmentation accuracy. Although a total of five segmentation models were assessed during initial testing, only two deep-learning models were ultimately employed for the final segmentation workflow described here. These were:

- **StarDist (v0.5.0)**, applied to the glioblastoma (GBM) surgical sample image
- **InstanSeg (v0.1.2)**, applied to the pancreas (training) surgical sample image

Before running the segmentation extensions in QuPath, image regions were annotated using the *Create Thresholder* feature. Because the aim was to detect all tissue, all biomarker channels were included, and the *average channels* option was used. As advised in the QuPath documentation, the analysis was performed at *Extremely Low Resolution*. Due to varying marker intensities, a threshold of **0.6** was applied for the training image and **3** for the GBM image, with a smoothing sigma of **3** (both QPTIFF images had a pixel type of `uint8` and pixel dimensions of **0.5062 × 0.5062 µm**).

A *minimum area threshold* of **20 µm<sup>2</sup>** was applied, corresponding to a circular object with a 5 µm diameter (π × 2.5<sup>2</sup> ≈ 19.6), to exclude small objects unlikely to represent intact nuclei or cells, particularly in regions where only DAPI channel expression was present and other markers were absent. Based on literature, the nucleus of a eukaryotic cell is typically 5–20 µm in diameter, supporting this cut-off. A *minimum hole area* of **10 µm<sup>2</sup>** was also set, meaning that internal gaps smaller than ≈ 3.6 µm in diameter were ignored as likely artefacts.

The **StarDist** single-channel (DAPI) model was applied to the GBM image using the pre-trained `dsb2018_heavy_augment.pb` model, with a tile (patch) size of **256 pixels**, matching the training dataset image size, a cell expansion radius of **5 µm**, and an object probability threshold of **0.5**.

The **InstanSeg** multi-channel model was applied to the training image using the pre-trained `fluorescence_nuclei_and_cells-0.1.0` model.  
For the training image, the channels used were: **DAPI, CD8, CD20, CD68, CD3e, Ki67, and pan-cytokeratin (pan-CK)**.  
For the GBM image, the channels used were: **DAPI, Olig2, SOX2, CD68, CD3e, and GFAP**.  
Custom settings included a tile size of **512 pixels** (recommended in the InstanSeg documentation), padding of **16 pixels** to avoid cutting cells at tile edges, and execution on CPU using **five threads**.

Both segmentation methods generated tabular datasets containing **numerical intensity values** for each biomarker, along with detected cell **X and Y centroid coordinates** and **morphological measurements** (including nucleus and cell areas). These values were used to phenotype individual cells through thresholding and analysis of marker expression profiles. By examining each cell’s expression pattern across the full marker panel, cell types were assigned, forming the basis for the broader phenotypic and spatial maps of the tumour microenvironment (TME).
