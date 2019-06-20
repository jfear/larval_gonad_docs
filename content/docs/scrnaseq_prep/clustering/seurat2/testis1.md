---
title: Seurat2 Testis 1 Clustering
author: Justin Fear
date: 'Compiled: June 20, 2019'
output:
    html_document:
        self_contained: TRUE
        keep_md: TRUE
---

## Testis 1

Here I explore clustering using Seurat v2 for clustering.

### Normalize Data

```r
sobj <- FindVariableGenes(
    sobj,
    mean.function = ExpMean,
    dispersion.function = LogVMR,
    x.low.cutoff = 0.0125,
    x.high.cutoff = 3,
    y.cutoff = 0.5,
    display.progress = FALSE
)
```

{{<figure src="../testis1_figures/find_variable_genes-1.png">}}

### Scale Data


```r
sobj <- ScaleData(
    sobj,
    vars.to.regress = "nUMI",
    display.progress = FALSE
)
```

### Dimensionality Reduction



#### Dimensional Gene Loadings

{{<figure src="../testis1_figures/dotplot_pcs-1.png" width="100%" >}}

#### Top Genes Heatmap

{{<figure src="../testis1_figures/heatmap_pca-1.png" width="100%" >}}

#### JackStraw



{{<figure src="../testis1_figures/jackstraw_pca-1.png" width="100%" >}}

```
## An object of class seurat in project testis1 
##  11575 genes across 2710 samples.
```

#### Elbow Plot

{{<figure src="../testis1_figures/elbow_pca-1.png" width="100%" >}}

#### Dimension Selection


### Clustering


```
## [1] "res.0.4:  9"
## [1] "res.0.5:  9"
## [1] "res.0.6:  9"
## [1] "res.0.7:  9"
## [1] "res.0.8:  13"
## [1] "res.0.9:  13"
## [1] "res.1:  13"
## [1] "res.1.1:  13"
## [1] "res.1.2:  15"
```

{{<figure src="../testis1_figures/umap-1.png" width="100%" >}}

{{<figure src="../testis1_figures/tsne-1.png" width="100%" >}}

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

| cluster | Number Markers Per Cluster |
|---------|----------------------------|
| 0       | 245                        |
| 1       | 218                        |
| 2       | 1174                       |
| 3       | 19                         |
| 4       | 485                        |
| 5       | 362                        |
| 6       | 110                        |
| 7       | 1008                       |
| 8       | 930                        |
| 9       | 966                        |
| 10      | 570                        |
| 11      | 1707                       |
| 12      | 702                        |

#### Top 12 Genes Per Cluster

##### Cluster 0
{{<figure src="../testis1_figures/plot_top_markers-1.png" width="100%" >}}

##### Cluster 1
{{<figure src="../testis1_figures/plot_top_markers-2.png" width="100%" >}}

##### Cluster 2
{{<figure src="../testis1_figures/plot_top_markers-3.png" width="100%" >}}

##### Cluster 3
{{<figure src="../testis1_figures/plot_top_markers-4.png" width="100%" >}}

##### Cluster 4
{{<figure src="../testis1_figures/plot_top_markers-5.png" width="100%" >}}

##### Cluster 5
{{<figure src="../testis1_figures/plot_top_markers-6.png" width="100%" >}}

##### Cluster 6
{{<figure src="../testis1_figures/plot_top_markers-7.png" width="100%" >}}

##### Cluster 7
{{<figure src="../testis1_figures/plot_top_markers-8.png" width="100%" >}}

##### Cluster 8
{{<figure src="../testis1_figures/plot_top_markers-9.png" width="100%" >}}

##### Cluster 9
{{<figure src="../testis1_figures/plot_top_markers-10.png" width="100%" >}}

##### Cluster 10
{{<figure src="../testis1_figures/plot_top_markers-11.png" width="100%" >}}

##### Cluster 11
{{<figure src="../testis1_figures/plot_top_markers-12.png" width="100%" >}}

##### Cluster 12
{{<figure src="../testis1_figures/plot_top_markers-13.png" width="100%" >}}
