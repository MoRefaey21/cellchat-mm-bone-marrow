# Cell-Cell Communication Analysis in Malignant Bone Marrow Using CellChat

A CellChat-based intercellular communication analysis of a malignant multiple myeloma bone marrow sample, using the annotated Seurat object from Assignment 1 as input. Completed as an individual assignment for the Single-Cell RNA-seq Analysis course, Postgraduate Bioinformatics Diploma at Nile University.

**Author:** Mohammed Refaey

**Dependency:** requires `checkpoints/seu_with_SingleR.rds`, produced by the Assignment 1 analysis in [seurat-mm-bone-marrow](https://github.com/MoRefaey21/seurat-mm-bone-marrow). Run that report first (the file is not committed).

## Question

What cell-cell communication networks are active in the malignant bone marrow microenvironment of multiple myeloma? Which signaling pathways dominate, which cell types act as signal senders versus receivers, and what do the MIF and GALECTIN pathways specifically reveal about immune evasion in this context?

## Pipeline

The annotated Seurat object from Assignment 1 (13 cell types, 3,789 cells) is converted to a CellChat object grouped by SingleR labels. The full human ligand-receptor database (`CellChatDB.human`, which includes Secreted Signaling, ECM-Receptor and Cell-Cell Contact interactions) is attached and the expression data are subset to the signaling genes in it with `subsetData`. Overexpressed genes and interactions are identified with `identifyOverExpressedGenes` and `identifyOverExpressedInteractions` (437 highly variable LR pairs detected). Communication probabilities are inferred with `computeCommunProb` (triMean method); low-cell groups (fewer than 10 cells) are filtered out with `filterCommunication`, removing 76.7% of interactions from 8 sparsely populated cell types (CMP, GMP, HSC-G-CSF, HSC_CD34+, Myelocyte, Neurons, Pro-B, Pro-Myelocyte). Pathway-level communication is aggregated with `computeCommunProbPathway` and network centrality scores computed with `netAnalysis_computeCentrality`. Visualizations include circle plots (interaction count and weight), interaction heatmaps, a sender-vs-receiver scatter plot, pathway-specific circle plots and bubble plots for MIF and GALECTIN, and gene-expression violin plots for pathway genes.

## Results

22 signaling pathways were inferred across the 5 cell types that passed the filter (B cells, monocytes, NK cells, pre-B cells, T cells), from 248 ligand-receptor interactions. MHC-I dominates the network by a wide margin, followed by MIF, CD99, CypA and MHC-II; GALECTIN ranks eighth.

- **MIF** (MIF to CD74 with CXCR4 or CD44) is sent mainly by B cells and T cells, with the strongest links from B cells to T cells and to monocytes, and T cell autocrine signaling. Monocytes appear as receivers of MIF rather than senders.
- **GALECTIN** (LGALS9 to CD44, CD45 and P4HB) comes almost entirely from monocytes, which signal to T cells, NK cells, pre-B cells and other monocytes. LGALS9 binding is one route by which myeloid cells can suppress T and NK cell activity, so this is a candidate immune-regulatory axis in this sample.
- **Signaling roles:** by CellChat's network centrality, NK cells are both the strongest senders and, by a wide margin, the strongest receivers, followed by T cells; monocytes send the least overall but are the main GALECTIN source.

The 76.7% of interactions removed by the cell-number filter reflects how few cells several types have in a single sample (8 types with fewer than 10 cells). With one patient sample, these patterns are descriptive and cannot be compared statistically.

## Tools

R with CellChat for the communication inference workflow; Seurat for object handling; ComplexHeatmap for interaction heatmaps; patchwork for plot composition.

## Repository contents

- `Assignment2_CellChat.qmd`: full analysis (Quarto source)
- `Assignment2_CellChat_Report.html`: rendered HTML report with all visualizations
- `CellChat_edges.csv`: inferred ligand-receptor interaction network
- `README.md`
- `LICENSE`
- `.gitignore`

## Data and dependencies

No separate data download is needed. The script loads `checkpoints/seu_with_SingleR.rds` produced by Assignment 1. Place that checkpoint in a `checkpoints/` folder at the project root before running.

## Reproducibility

Open `Assignment2_CellChat.qmd` in RStudio with the working directory set to the project root. The script creates `write/figures/` and `write/tables/` on first run (`CellChat_edges.csv` is written to `write/tables/`). The final CellChat object is saved as `cellchat.rds` for further exploration.
