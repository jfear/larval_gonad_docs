---
title: Testis 1 Individual Clustering
author: Justin Fear
date: 'Compiled: June 24, 2019'
weight: 2
---

<span style="font-size: 3em; font-weight: 'strong'; color: #ff0000;">UNDER CONSTRUCTION</span>

## Testis 1 Individual Clustering

### Normalize Data

```r
sobj <- FindVariableFeatures(
    sobj,
    selection.method = "vst",
    nfeatures = 2000,
    verbose = FALSE
)

top20 <- head(VariableFeatures(sobj), 20)
```

{{<figure src="../testis1_figures/scatter_variable_genes-1.png" width="100%" >}}

### Scale Data

```r
all_genes <- rownames(sobj)

sobj <- ScaleData(
    sobj,
    features = all_genes,
    vars.to.regress = "nCount_RNA",
    verbose = FALSE
)
```

### Dimensionality Reduction

#### Dimensional Gene Loadings

{{<figure src="../testis1_figures/dotplot_pcs-1.png" width="100%" >}}

#### Top Genes Heatmap

{{<figure src="../testis1_figures/heatmap_pca-1.png" width="100%" >}}

#### JackStraw

{{<figure src="../testis1_figures/jackstraw_pca-1.png" width="100%" >}}

#### Elbow Plot

{{<figure src="../testis1_figures/elbow_pca-1.png" width="100%" >}}

#### Dimension Selection

### Clustering

| Resolution | Num of Clusters |
|------------|----------------:|
| 0.4        |              10 |
| 0.5        |              10 |
| 0.6        |              11 |
| 0.7        |              12 |
| **0.8**    |          **13** |
| 0.9        |              14 |
| 1          |              14 |
| 1.1        |              16 |
| 1.2        |              16 |

{{<figure src="../testis1_figures/umap-1.png" width="100%" >}}

### Look at marker genes

#### Spermatogonia

{{<figure src="../testis1_figures/heatmap_gonia-1.png" width="100%" >}}

{{<figure src="../testis1_figures/heatmap_gonia-2.png" width="100%" >}}

#### Spermatocytes

{{<figure src="../testis1_figures/heatmap_spermatocytes-1.png" width="100%" >}}

{{<figure src="../testis1_figures/heatmap_spermatocytes-2.png" width="100%" >}}

#### CySC

{{<figure src="../testis1_figures/heatmap_cysc-1.png" width="100%" >}}

{{<figure src="../testis1_figures/heatmap_cysc-2.png" width="100%" >}}

#### TE

{{<figure src="../testis1_figures/heatmap_te-1.png" width="100%" >}}

{{<figure src="../testis1_figures/heatmap_te-2.png" width="100%" >}}

#### PC

{{<figure src="../testis1_figures/heatmap_pc-1.png" width="100%" >}}

{{<figure src="../testis1_figures/heatmap_pc-2.png" width="100%" >}}

#### hub

{{<figure src="../testis1_figures/heatmap_hub-1.png" width="100%" >}}

{{<figure src="../testis1_figures/heatmap_hub-2.png" width="100%" >}}

### Cluster Markers

| cluster | Num Biomarkers per Cluster |
|---------|---------------------------:|
| 0       |                        130 |
| 1       |                        617 |
| 2       |                        263 |
| 3       |                      2,043 |
| 4       |                        495 |
| 5       |                        143 |
| 6       |                        455 |
| 7       |                      1,175 |
| 8       |                      1,266 |
| 9       |                          9 |
| 10      |                      1,029 |
| 11      |                      1,031 |
| 12      |                        633 |