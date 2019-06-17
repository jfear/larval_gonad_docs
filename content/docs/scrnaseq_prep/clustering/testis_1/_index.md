---
title: Seurat v3 Testis 1 Clustering
author: Justin Fear
date: 'Compiled: June 17, 2019'
---

Here I explore clustering testis1 using Seurat v3. This document will walk through the clustering process and highlights different decision points.

## Quality Control and Filtering

First I identify additional cells to remove prior to analysis.
In particular, I want to remove GEMs that are possibly homotypic doublets as well as dead or dying cells.
Homotypic doublets will have abnormally high gene/UMI content.
Apoptotic cells will likely have high mitochondrial gene expression.
I also look for cells with high rRNA expression which may have had bad poly(A) selection.
However, both high RNA content and mitochondrial expression could be functionally important in germline development.
I would like to remove problematic cells, but not be too stringent to remove real signal.

```r
VlnPlot(sobj, features = c("nFeature_RNA", "nCount_RNA", "percent.mt", "percent.ribo"), pt.size = .1, ncol = 4)
```

{{<figure src="testis1_figures/dist_basic_cnts-1.png" width="100%">}}

```r
p1 <- FeatureScatter(sobj, feature1 = "nCount_RNA", feature2 = "percent.mt") + scale_x_log10() + xlab("UMI")
p2 <- FeatureScatter(sobj, feature1 = "nCount_RNA", feature2 = "percent.ribo") + scale_x_log10() + xlab("UMI")
p3 <- FeatureScatter(sobj, feature1 = "nCount_RNA", feature2 = "nFeature_RNA") + scale_x_log10() + xlab("UMI") + ylab("Number Genes")
CombinePlots(plots = list(p1, p2, p3), ncol = 3)
```

{{<figure src="testis1_figures/scatter_basic_cnts-1.png" width="100%">}}

To identify problematic cells I plot several key features.

* `nFeature_RNA` is the number of expressed genes per cell
* `nCount_RNA` is the total UMI per cell
* `percent.mt` is the proportion of expressed genes that are mitochondrial.
* `percent.ribo` is the proportion of expressed genes that are rRNA.

I see a break in the distribution of expressed genes around 4,000 genes (`nFeature_RNA` distribution panel) .
Gene content is correlated (0.84) with total UMI (last scatter plot), so removing these cells would result in removal of cells above 50,000 UMI.
There are also a number of lower expressing cells that have a high percent (>40%) of mitochondrial reads.
rRNA content is low for most cells (<2%) suggesting overall good poly(A) selection.

### Filtering criteria

* 200 < Number of Genes <= 4,000
* percent.mt <= 40
* percent.mt <= 4

```r
num_cells <- dim(sobj)[2]
sobj <- subset(sobj, subset = nFeature_RNA > 200 & nFeature_RNA <= 4000 & percent.mt <= 40 & percent.ribo <= 4)
num_cells_kept <- dim(sobj)[2]
num_cells_removed <- num_cells - num_cells_kept
num_genes <- dim(sobj)[1]
```

### Filtering Summary 

* Starting with 2,741 cells.
* Removing 67 cells.
* Keeping 2,674 cells.
* Using 11,956 genes.

## Normalization and Scaling

I am using Seurat's default LogNormalization followed by a variance stabelizing transformation and selection of highly variable genes.
Unlike previous v2, Seurat3 is using more variable genes (n = 2,000).
Variable genes are in red, with the top 20 variable genes annotated.
I will use these genes 2,000 genes as features during dimensionality reduction.

```r
p1 <- VariableFeaturePlot(sobj)
p2 <- LabelPoints(plot = p1, points = top20, repel = TRUE)
print(p2)
```

{{<figure src="testis1_figures/scatter_variable_genes-1.png" width="100%">}}

Similar to Seurat v2, I decided to regress out mitochondrial batch effects during scaling.

```r
all_genes <- rownames(sobj)
sobj <- ScaleData(sobj, features = all_genes, vars.to.regress = "percent.mt", verbose = FALSE)
```

## Dimensionality Reduction

I used PCA to reduce dimensionality using the 2,000 variable genes.
To decide how many PCs to use I look at a number of diagnostic plots.

### Dimensional Gene Loadings

{{<figure src="testis1_figures/dotplot_pcs-1.png" width="100%">}}

There are some interesting genes poping out in the first 4 PCs.

* PC1
    * *soti*
    * *dj*
    * *ocn*
* PC2
    * *br*
* PC3
    * *Phf7*
    * *vas*
* PC4
    * *Abd-B*
    * *Fas3*

### Top Genes Heatmap

The first metric plotting each PC with the top genes.
On the X-axis are all of the cells and on the Y-axis are the top genes driving that variation in that PC.
I am looking through each PC and seeing if there are genes that I recognize.
The idea being that I don't want to remove a PC that has interesting genes driving variation.
I start at the last PC and working my way up.
PC 12 is the first to have a gene that I recognize (*nht*).
This would suggest a cutoff around PC12.

{{<figure src="testis1_figures/heatmap_pca-1.png" width="100%">}}

### JackStraw

The next method is an actual metric called JackStraw analsyis.
In this plot we look for a large drop in the theortical vs empirical distribution.
For sure we don't want to include any PCs that fall below the dashed line.
However, it is suggested to remove PCs after the first major drop which puts us around PC11.

{{<figure src="testis1_figures/jackstraw_pca-1.png" width="100%">}}

### Elbow Plot

The elbow plot is a classic metric, plotted on the Y-axis is the variability measured by standard deviation.
On the X axis is the first 20 PCs.
Similar to the Jackstraw plot we want to look for a large space between points and a flattening out of the captured variation.
This metric would suggest somewhere between PCs 6-12

{{<figure src="testis1_figures/elbow_pca-1.png" width="100%">}}

### Dimension Selection

After looking at all of the metrics I think 11 is a good point to cut this sample off.

## Clustering

Next I perform clustering.
Since I am wanting to compare methods, and parameters can be adjusted to get any number of clusters, I have decided to shoot for 12-13 clusters.
From preliminary analysis we think that we capture roughly this many cell types.
I ran a range of resolutions and found for this sample around `0.8` give me 13 clusters.

{{<figure src="testis1_figures/umap-1.png" width="100%">}}

{{<figure src="testis1_figures/tsne-1.png" width="100%">}}

## Look at marker genes

To get a better sense of these clusters I am plotting various known marker genes from the literature.

### Spermatogonia

*bam* looks to be expressed in cluster 7 along with *Rbp4*, *Phf7*, and *fest*.
In general, the bill of the duck shape looks to be goina.

{{<figure src="testis1_figures/heatmap_gonia-1.png" width="100%">}}

{{<figure src="testis1_figures/heatmap_gonia-2.png" width="100%">}}

### Spermatocytes

Cluster 11 has good expression of *sa*, *bol*, *fzo*, *mia*, *tomb*, *d-cup*, *bb8*.
The head of the duck looks like spermatocytes.

{{<figure src="testis1_figures/heatmap_spermatocytes-1.png" width="100%">}}

{{<figure src="testis1_figures/heatmap_spermatocytes-2.png" width="100%">}}

### CySC

Somatic cells appear to be the ducsk body, with *tj* in the very tip tail.

{{<figure src="testis1_figures/heatmap_cysc-1.png" width="100%">}}

{{<figure src="testis1_figures/heatmap_cysc-2.png" width="100%">}}

### TE

The TE appears to be the large island.

{{<figure src="testis1_figures/heatmap_te-1.png" width="100%">}}

{{<figure src="testis1_figures/heatmap_te-2.png" width="100%">}}

### PC

PC appears to be the smaller island.

{{<figure src="testis1_figures/heatmap_pc-1.png" width="100%">}}

{{<figure src="testis1_figures/heatmap_pc-2.png" width="100%">}}

### hub

The hub genes overlap the TE island and some of the cells in the tail.

{{<figure src="testis1_figures/heatmap_hub-1.png" width="100%">}}

{{<figure src="testis1_figures/heatmap_hub-2.png" width="100%">}}

## Cluster Markers

Next I identified a number of genes that are differentially upregulated in each cluster.
These genes can be used as markers that that cluster.
Cluster 5 only had 20 genes, while the other clusters had `>=100`

| cluster | num_markers_per_cluster |
|---------|-------------------------|
| 0       | 120                     |
| 1       | 374                     |
| 2       | 428                     |
| 3       | 18                      |
| 4       | 1413                    |
| 5       | 576                     |
| 6       | 450                     |
| 7       | 1193                    |
| 8       | 980                     |
| 9       | 854                     |
| 10      | 99                      |
| 11      | 963                     |
| 12      | 2184                    |

Here I plot the top 12 genes for each cluster.

{{<figure src="testis1_figures/plot_top_markers-1.png" width="100%">}}

{{<figure src="testis1_figures/plot_top_markers-2.png" width="100%">}}

{{<figure src="testis1_figures/plot_top_markers-3.png" width="100%">}}

{{<figure src="testis1_figures/plot_top_markers-4.png" width="100%">}}

{{<figure src="testis1_figures/plot_top_markers-5.png" width="100%">}}

{{<figure src="testis1_figures/plot_top_markers-6.png" width="100%">}}

{{<figure src="testis1_figures/plot_top_markers-7.png" width="100%">}}

{{<figure src="testis1_figures/plot_top_markers-8.png" width="100%">}}

{{<figure src="testis1_figures/plot_top_markers-9.png" width="100%">}}

{{<figure src="testis1_figures/plot_top_markers-10.png" width="100%">}}

{{<figure src="testis1_figures/plot_top_markers-11.png" width="100%">}}

{{<figure src="testis1_figures/plot_top_markers-12.png" width="100%">}}

{{<figure src="testis1_figures/plot_top_markers-13.png" width="100%">}}
