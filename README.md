# Cell-Cell Communication Analysis in Malignant Bone Marrow Using CellChat

A CellChat-based intercellular communication analysis of a malignant multiple myeloma bone marrow sample, using the annotated Seurat object from Assignment 1 as input. Completed as an individual assignment for the Single-Cell RNA-seq Analysis course, Postgraduate Bioinformatics Diploma at Nile University.

**Author:** Mohammed Ashraf

**Dependency:** requires `checkpoints/seu_with_SingleR.rds` from Assignment 1.

## Question

What cell-cell communication networks are active in the malignant bone marrow microenvironment of multiple myeloma? Which signaling pathways dominate, which cell types act as signal senders versus receivers, and what do the MIF and GALECTIN pathways specifically reveal about immune evasion in this context?

## Pipeline

The annotated Seurat object from Assignment 1 (13 cell types, 3,789 cells) is converted to a CellChat object grouped by SingleR labels. The human ligand-receptor database (`CellChatDB.human`) is attached and expression data subset to secreted signaling genes. Overexpressed genes and interactions are identified with `identifyOverExpressedGenes` and `identifyOverExpressedInteractions` (437 highly variable LR pairs detected). Communication probabilities are inferred with `computeCommunProb` (triMean method); low-cell groups (fewer than 10 cells) are filtered out with `filterCommunication`, removing 76.7% of interactions from 8 sparsely populated cell types (CMP, GMP, HSC-G-CSF, HSC_CD34+, Myelocyte, Neurons, Pro-B, Pro-Myelocyte). Pathway-level communication is aggregated with `computeCommunProbPathway` and network centrality scores computed with `netAnalysis_computeCentrality`. Visualizations include circle plots (interaction count and weight), interaction heatmaps, a sender-vs-receiver scatter plot, pathway-specific circle plots and bubble plots for MIF and GALECTIN, and gene-expression violin plots for pathway genes.

## Results

22 signaling pathways were inferred across the remaining 5 cell types (B cells, Monocytes, NK cells, Pre-B cells, T cells). MHC-I, MIF, CD99, and GALECTIN are among the most active pathways. MIF signaling (MIF-CD74-CXCR4/CD44) is enriched in monocyte-to-T cell and monocyte-to-B cell interactions, consistent with M2-like macrophage polarization and T cell suppression characteristic of the myeloma niche. GALECTIN signaling (LGALS9-CD44/CD45) is active across lymphoid populations and aligns with known T cell exhaustion mechanisms. T cells and monocytes act as dominant senders; NK cells and monocytes receive the most incoming signal. The high fraction of excluded cell types (76.7%) reflects the limited number of cells per type in a single-sample dataset and is an expected limitation.

## Tools

R with CellChat for the communication inference workflow; Seurat for object handling; ComplexHeatmap for interaction heatmaps; patchwork for plot composition.

## Repository contents

- `Assignment2_CellChat.qmd`: full analysis (Quarto source)
- `Assignment2_CellChat_Report.html`: rendered HTML report with all visualizations
- `CellChat_edges.csv`: inferred ligand-receptor interaction network
- `README.md`
- `.gitignore`

## Data and dependencies

No separate data download is needed. The script loads `checkpoints/seu_with_SingleR.rds` produced by Assignment 1. Place that checkpoint in a `checkpoints/` folder at the project root before running.

## Reproducibility

Open `Assignment2_CellChat__1_.qmd` in RStudio with the working directory set to the project root. The script creates `write/figures/`, `write/tables/`, and `checkpoints/` on first run. The final CellChat object is saved as `cellchat.rds` for further exploration.
