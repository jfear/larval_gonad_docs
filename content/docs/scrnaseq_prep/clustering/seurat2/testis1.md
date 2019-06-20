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

![](testis1_figures/find_variable_genes-1.png)<!-- -->

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

![](testis1_figures/dotplot_pcs-1.png)<!-- -->

#### Top Genes Heatmap

![](testis1_figures/heatmap_pca-1.png)<!-- -->

#### JackStraw



![](testis1_figures/jackstraw_pca-1.png)<!-- -->

```
## An object of class seurat in project testis1 
##  11575 genes across 2710 samples.
```

#### Elbow Plot

![](testis1_figures/elbow_pca-1.png)<!-- -->

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

![](testis1_figures/umap-1.png)<!-- -->

![](testis1_figures/tsne-1.png)<!-- -->

### Look at marker genes



#### Spermatogonia

![](testis1_figures/heatmap_gonia-1.png)<!-- -->![](testis1_figures/heatmap_gonia-2.png)<!-- -->

#### Spermatocytes

![](testis1_figures/heatmap_spermatocytes-1.png)<!-- -->![](testis1_figures/heatmap_spermatocytes-2.png)<!-- -->

#### CySC

![](testis1_figures/heatmap_cysc-1.png)<!-- -->![](testis1_figures/heatmap_cysc-2.png)<!-- -->

#### TE

![](testis1_figures/heatmap_te-1.png)<!-- -->![](testis1_figures/heatmap_te-2.png)<!-- -->

#### PC

![](testis1_figures/heatmap_pc-1.png)<!-- -->![](testis1_figures/heatmap_pc-2.png)<!-- -->

#### hub

![](testis1_figures/heatmap_hub-1.png)<!-- -->![](testis1_figures/heatmap_hub-2.png)<!-- -->

### Cluster Markers




```
## # A tibble: 13 x 2
##    cluster num_markers_per_cluster
##    <fct>                     <int>
##  1 0                           245
##  2 1                           218
##  3 2                          1174
##  4 3                            19
##  5 4                           485
##  6 5                           362
##  7 6                           110
##  8 7                          1008
##  9 8                           930
## 10 9                           966
## 11 10                          570
## 12 11                         1707
## 13 12                          702
```

#### Top 12 Genes Per Cluster



![](testis1_figures/plot_top_markers-1.png)<!-- -->![](testis1_figures/plot_top_markers-2.png)<!-- -->![](testis1_figures/plot_top_markers-3.png)<!-- -->![](testis1_figures/plot_top_markers-4.png)<!-- -->![](testis1_figures/plot_top_markers-5.png)<!-- -->![](testis1_figures/plot_top_markers-6.png)<!-- -->![](testis1_figures/plot_top_markers-7.png)<!-- -->![](testis1_figures/plot_top_markers-8.png)<!-- -->![](testis1_figures/plot_top_markers-9.png)<!-- -->![](testis1_figures/plot_top_markers-10.png)<!-- -->![](testis1_figures/plot_top_markers-11.png)<!-- -->![](testis1_figures/plot_top_markers-12.png)<!-- -->![](testis1_figures/plot_top_markers-13.png)<!-- -->






