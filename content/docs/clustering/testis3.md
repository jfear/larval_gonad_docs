---
title: Testis 3 Individual Clustering
author: Justin Fear
date: 'Compiled: June 24, 2019'
weight: 4
---

<span style="font-size: 3em; font-weight: 'strong'; color: #ff0000;">UNDER CONSTRUCTION</span>

## Testis 3 Individual Clustering

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

{{<figure src="../testis3_figures/scatter_variable_genes-1.png" width="100%" >}}

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

{{<figure src="../testis3_figures/dotplot_pcs-1.png" width="100%" >}}

#### Top Genes Heatmap

{{<figure src="../testis3_figures/heatmap_pca-1.png" width="100%" >}}

#### JackStraw

{{<figure src="../testis3_figures/jackstraw_pca-1.png" width="100%" >}}

#### Elbow Plot

{{<figure src="../testis3_figures/elbow_pca-1.png" width="100%" >}}

#### Dimension Selection

### Clustering

| Resolution | Num of Clusters |
|------------|----------------:|
| 0.4        |              11 |
| 0.5        |              13 |
| 0.6        |              13 |
| 0.7        |              15 |
| 0.8        |              16 |
| 0.9        |              17 |
| 1          |              19 |
| 1.1        |              18 |
| 1.2        |              21 |

{{<figure src="../testis3_figures/umap-1.png" width="100%" >}}

{{<figure src="../testis3_figures/tsne-1.png" width="100%" >}}

### Look at marker genes

#### Spermatogonia

{{<figure src="../testis3_figures/heatmap_gonia-1.png" width="100%" >}}

{{<figure src="../testis3_figures/heatmap_gonia-2.png" width="100%" >}}

#### Spermatocytes

{{<figure src="../testis3_figures/heatmap_spermatocytes-1.png" width="100%" >}}

{{<figure src="../testis3_figures/heatmap_spermatocytes-2.png" width="100%" >}}

#### CySC

{{<figure src="../testis3_figures/heatmap_cysc-1.png" width="100%" >}}

{{<figure src="../testis3_figures/heatmap_cysc-2.png" width="100%" >}}

#### TE

{{<figure src="../testis3_figures/heatmap_te-1.png" width="100%" >}}

{{<figure src="../testis3_figures/heatmap_te-2.png" width="100%" >}}

#### PC

{{<figure src="../testis3_figures/heatmap_pc-1.png" width="100%" >}}

{{<figure src="../testis3_figures/heatmap_pc-2.png" width="100%" >}}

#### hub

{{<figure src="../testis3_figures/heatmap_hub-1.png" width="100%" >}}

{{<figure src="../testis3_figures/heatmap_hub-2.png" width="100%" >}}

### Cluster Markers

| cluster | Num Biomarkers per Cluster |
|---------|---------------------------:|
| 0       |                        160 |
| 1       |                        786 |
| 2       |                        567 |
| 3       |                        344 |
| 4       |                         65 |
| 5       |                      1,486 |
| 6       |                        453 |
| 7       |                      1,355 |
| 8       |                        844 |
| 9       |                        801 |
| 10      |                        364 |
| 11      |                      1,002 |
| 12      |                      1,390 |
