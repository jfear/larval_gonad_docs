---
title: Testis 2 Individual Clustering
author: Justin Fear
date: 'Compiled: June 24, 2019'
weight: 3
---

<span style="font-size: 3em; font-weight: 'strong'; color: #ff0000;">UNDER CONSTRUCTION</span>

## Testis 2 Individual Clustering

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

{{<figure src="../testis2_figures/scatter_variable_genes-1.png" width="100%" >}}

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

{{<figure src="../testis2_figures/dotplot_pcs-1.png" width="100%" >}}

#### Top Genes Heatmap

{{<figure src="../testis2_figures/heatmap_pca-1.png" width="100%" >}}

#### JackStraw

{{<figure src="../testis2_figures/jackstraw_pca-1.png" width="100%" >}}

#### Elbow Plot

{{<figure src="../testis2_figures/elbow_pca-1.png" width="100%" >}}

#### Dimension Selection

### Clustering


| Resolution | Num of Clusters |
|------------|----------------:|
| 0.4        |              10 |
| 0.5        |              12 |
| **0.6**    |          **14** |
| 0.7        |              15 |
| 0.8        |              16 |
| 0.9        |              17 |
| 1          |              18 |
| 1.1        |              20 |
| 1.2        |              20 |

{{<figure src="../testis2_figures/umap-1.png" width="100%" >}}

### Look at marker genes

#### Spermatogonia

{{<figure src="../testis2_figures/heatmap_gonia-1.png" width="100%" >}}

{{<figure src="../testis2_figures/heatmap_gonia-2.png" width="100%" >}}

#### Spermatocytes

{{<figure src="../testis2_figures/heatmap_spermatocytes-1.png" width="100%" >}}

{{<figure src="../testis2_figures/heatmap_spermatocytes-2.png" width="100%" >}}

#### CySC

{{<figure src="../testis2_figures/heatmap_cysc-1.png" width="100%" >}}

{{<figure src="../testis2_figures/heatmap_cysc-2.png" width="100%" >}}

#### TE

{{<figure src="../testis2_figures/heatmap_te-1.png" width="100%" >}}

{{<figure src="../testis2_figures/heatmap_te-2.png" width="100%" >}}

#### PC

{{<figure src="../testis2_figures/heatmap_pc-1.png" width="100%" >}}

{{<figure src="../testis2_figures/heatmap_pc-2.png" width="100%" >}}

#### hub

{{<figure src="../testis2_figures/heatmap_hub-1.png" width="100%" >}}

{{<figure src="../testis2_figures/heatmap_hub-2.png" width="100%" >}}

### Cluster Markers

| cluster | Num Biomarkers per Cluster |
|---------|---------------------------:|
| 0       |                        359 |
| 1       |                        392 |
| 2       |                        683 |
| 3       |                      1,375 |
| 4       |                        556 |
| 5       |                        377 |
| 6       |                      1,615 |
| 7       |                        501 |
| 8       |                      1,088 |
| 9       |                      1,918 |
| 10      |                        286 |
| 11      |                        570 |
| 12      |                      1,933 |
| 13      |                         74 |
