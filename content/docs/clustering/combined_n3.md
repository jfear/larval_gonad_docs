---
title: Testis Integrated Clustering (n = 3)
author: Justin Fear
date: 'Compiled: June 24, 2019'
output: 
    html_document:
        self_contained: TRUE
        keep_md: TRUE
---

## Testis Integrated Clustering (n = 3; Seurat v3)

### UMAP Projection Clustering From Individual Replicates

First I clustered each replicate separately using a replicate specific workflow.
For consistency, I called 12-13 clusters for each replicate. 
Cluster numbers and colors do not correspond. 
For more details see individual clustering analysis.

{{<figure src="../combined_n3_figures/dimplots-1.png" width="100%">}}

### UMAP Projection Clustering From Integrated Replicates

Integrated analysis begins by taking normalized reads from each replicate and aligning cells using canonical correlation analysis (CCA).
Cells were clustered using Seurat v3. 
Individual sample clustering shows better separation of germ cells and somatic cells, but overall structure is maintained in the integrated analysis.
The largest cluster (0) had 3,217 cells while the smallest cluster (9) had 853 cells.
Cells from each replicate are present in each cluster.

{{<figure src="../combined_n3_figures/umap_reps-1.png" width="100%">}}


{{<figure src="../combined_n3_figures/umap_reps_split-1.png" width="100%">}}


**Table 1. Number of cells per integrated cluster.**

| Cluster | Num. Cells Per Cluster |
|---------|-----------------------:|
| 0       |                  3,217 |
| 1       |                  3,149 |
| 2       |                  2,652 |
| 3       |                  2,388 |
| 4       |                  1,510 |
| 5       |                  1,466 |
| 6       |                  1,401 |
| 7       |                  1,376 |
| 8       |                    870 |
| 9       |                    853 |

<!-- TODO add plot with number cells per rep by cluster -->


### Literature Genes

To annotate clusters I am using genes from the literature. 
Plotted below are UMAP projects for genes from the literature. 
I also have violin plots showing the distribution of expression for each gene.
For some cell types I show a "Zoomed" view of a few key genes.

#### Spermatogonia (Cluster 9)

{{< tabs "spermatogoina" >}}
{{< tab "UMAP (Zoom)" >}}
{{<figure src="../combined_n3_figures/vas_bam-1.png" width="100%">}}
{{< /tab >}}
{{< tab "UMAP" >}}
{{<figure src="../combined_n3_figures/heatmap_gonia-2.png" width="100%">}}
{{< /tab >}}
{{< tab "Violin" >}}
{{<figure src="../combined_n3_figures/heatmap_gonia-1.png" width="100%">}}
{{< /tab >}}
{{< /tabs >}}

#### Spermatocytes (Clusters 0, 2, 5, 6)

{{< tabs "spermatocytes" >}}
{{< tab "UMAP (Zoom)" >}}
{{<figure src="../combined_n3_figures/aly_nht_can_soti-1.png" width="100%">}}
{{< /tab >}}
{{< tab "UMAP" >}}
{{<figure src="../combined_n3_figures/heatmap_spermatocytes-2.png" width="100%">}}
{{< /tab >}}
{{< tab "Violin" >}}
{{<figure src="../combined_n3_figures/heatmap_spermatocytes-1.png" width="100%">}}
{{< /tab >}}
{{< /tabs >}}

#### CySC (Clusters 1, 3, 4)

{{< tabs "cysc" >}}
{{< tab "UMAP (Zoom)" >}}
{{<figure src="../combined_n3_figures/tj_eya-1.png" width="100%">}}
{{< /tab >}}
{{< tab "UMAP" >}}
{{<figure src="../combined_n3_figures/heatmap_cysc-2.png" width="100%">}}
{{< /tab >}}
{{< tab "Violin" >}}
{{<figure src="../combined_n3_figures/heatmap_cysc-1.png" width="100%">}}
{{< /tab >}}
{{< /tabs >}}

#### TE (Cluster 7)

{{< tabs "te" >}}
{{< tab "UMAP" >}}
{{<figure src="../combined_n3_figures/heatmap_te-2.png" width="100%">}}
{{< /tab >}}
{{< tab "Violin" >}}
{{<figure src="../combined_n3_figures/heatmap_te-1.png" width="100%">}}
{{< /tab >}}
{{< /tabs >}}

#### Pigmet Cells (Cluster 8)

{{< tabs "pc" >}}
{{< tab "UMAP" >}}
{{<figure src="../combined_n3_figures/heatmap_pc-2.png" width="100%">}}
{{< /tab >}}
{{< tab "Violin" >}}
{{<figure src="../combined_n3_figures/heatmap_pc-1.png" width="100%">}}
{{< /tab >}}
{{< /tabs >}}

#### Hub (??)

{{< tabs "hub" >}}
{{< tab "UMAP" >}}
{{<figure src="../combined_n3_figures/heatmap_hub-2.png" width="100%">}}
{{< /tab >}}
{{< tab "Violin" >}}
{{<figure src="../combined_n3_figures/heatmap_hub-1.png" width="100%">}}
{{< /tab >}}
{{< /tabs >}}



