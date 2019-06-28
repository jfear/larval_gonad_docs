---
title: Testis 4 Individual Clustering
author: Justin Fear
date: 'Compiled: June 24, 2019'
weight: 5
---

<span style="font-size: 3em; font-weight: 'strong'; color: #ff0000;">UNDER CONSTRUCTION</span>

## Testis 4 Individual Clustering

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

{{<figure src="../testis4_figures/scatter_variable_genes-1.png" width="100%" >}}

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

{{<figure src="../testis4_figures/dotplot_pcs-1.png" width="100%" >}}

#### Top Genes Heatmap

{{<figure src="../testis4_figures/heatmap_pca-1.png" width="100%" >}}

#### JackStraw

{{<figure src="../testis4_figures/jackstraw_pca-1.png" width="100%" >}}

#### Elbow Plot

{{<figure src="../testis4_figures/elbow_pca-1.png" width="100%" >}}

#### Dimension Selection

### Clustering

| Resolution | Num of Clusters |
|------------|----------------:|
| 0.4        |              10 |
| 0.5        |              12 |
| **0.6**    |          **12** |
| 0.7        |              14 |
| 0.8        |              17 |
| 0.9        |              19 |
| 1          |              19 |
| 1.1        |              22 |
| 1.2        |              22 |

{{<figure src="../testis4_figures/umap-1.png" width="100%" >}}

### Look at marker genes

#### Spermatogonia

{{<figure src="../testis4_figures/heatmap_gonia-1.png" width="100%" >}}

{{<figure src="../testis4_figures/heatmap_gonia-2.png" width="100%" >}}

#### Spermatocytes

{{<figure src="../testis4_figures/heatmap_spermatocytes-1.png" width="100%" >}}

{{<figure src="../testis4_figures/heatmap_spermatocytes-2.png" width="100%" >}}

#### CySC

{{<figure src="../testis4_figures/heatmap_cysc-1.png" width="100%" >}}

{{<figure src="../testis4_figures/heatmap_cysc-2.png" width="100%" >}}

#### TE

{{<figure src="../testis4_figures/heatmap_te-1.png" width="100%" >}}

{{<figure src="../testis4_figures/heatmap_te-2.png" width="100%" >}}

#### PC

{{<figure src="../testis4_figures/heatmap_pc-1.png" width="100%" >}}

{{<figure src="../testis4_figures/heatmap_pc-2.png" width="100%" >}}

#### hub

{{<figure src="../testis4_figures/heatmap_hub-1.png" width="100%" >}}

{{<figure src="../testis4_figures/heatmap_hub-2.png" width="100%" >}}

### Cluster Markers

| cluster | Num Biomarkers per Cluster |
|---------|---------------------------:|
| 0       |                        105 |
| 1       |                        927 |
| 2       |                        536 |
| 3       |                        120 |
| 4       |                        560 |
| 5       |                      1,162 |
| 6       |                        786 |
| 7       |                      1,167 |
| 8       |                      2,188 |
| 9       |                      1,652 |
| 10      |                        719 |
| 11      |                      1,090 |
