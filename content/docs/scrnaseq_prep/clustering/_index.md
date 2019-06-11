---
title: "Cell Clustering"
weight: 10
# bookFlatSection: false
# bookShowToC: true
---

<span style="font-size: 3em; font-weight: 'strong'; color: #ff0000;">WORK IN PROGRESS DO NOT READ</span>

# Cell Clustering
Cell clustering is the next step after demultiplexing and alignment.
There are a number of tools created for scRNA-Seq analysis.
The two most popular tools are [`Seurat`](https://satijalab.org/seurat/) and [`Monocle`](http://cole-trapnell-lab.github.io/monocle-release/).
Both tools are very similar and follow the generate approach.

1. Cells are further filtered based on gene number and low/upper UMI cutoffs.
2. Dimensionality is reduced.
    - This can include gene filtering (features)
    - Also PCA like approaches to selected *N* orthogonal dimensions.
3. A K Nearest Neighbors (KNN) graph is constructed.
4. The [Louvain algorithm](https://en.wikipedia.org/wiki/Louvain_Modularity), or something similar, is used to identify sub module in the graph to call clusters (communities)
5. Differential expression among clusters helps to annotate clusters as a specific cell type.

None of the steps are deterministic.
We expect slight differences among approaches as well as slight different even among runs.
Below I summarize results from different methods and parameterization.

Currently there are no methods that correctly model scRNA-Seq replicates. 
The best method I could find was using [canonical correlation analysis (CCA)](https://en.wikipedia.org/wiki/Canonical_correlation). 
CCA tries to find the vectors that maximize the correlation among two data sets.
`Seurat` implements a form of CCA that can handle multiple samples. 
We use CCA to "align" our three replicates and then call clusters on the combined cells. 

## Seurat (lower cutoff)
{{% button href="https://github.com/jfear/larval_gonad/blob/5f8f4569ea253ab96d497055e7ae6ebb4c82a744/scrnaseq-wf/scripts/scrnaseq_combine_force.Rmd" %}}Code{{% /button %}}

For each replicate, I removed genes that were no expressed in ≥3 cells and I removed cells that did not have ≥200 expressed genes.
I then identified variable genes in each replicate using [`Seurat::FindVariableGenes`](https://github.com/satijalab/seurat/blob/64e73ee19d4462aaa7a1b51cc62d113438bf2f3f/R/preprocessing.R#L677-L850). 

| Replicate | Number Variable Genes |
|-----------|-----------------------|
| 1         | 576                   |
| 2         | 494                   |
| 3         | 330                   |
| 1 ∩ 2 ∩ 3 | 108                   |

Next I ran CCA using the 108 common variable genes.
Using 25 CCA dimensions I aligned the three replicates. 
Finally I use [`Seurat::FindClusters`](https://github.com/satijalab/seurat/blob/64e73ee19d4462aaa7a1b51cc62d113438bf2f3f/R/cluster_determination.R#L3-L178) to call clusters in 25 dimension Aligned CCA space.
I used three different resolutions for clustering finding (0.4, 0.6, 1).

We decided to use `res=0.6` for downstream analysis.

{{<figure src="tsne.svg" width="100%">}}


### Resolution 0.4

{{<figure src="tsne_seurat_low_res0.4.png">}}

When using a resolution of 0.4 we obtained 9 clusters.


### Resolution 0.6

{{<figure src="tsne_seurat_low_res0.6.png">}}


### Resolution 1

{{<figure src="tsne_seurat_low_res1.png">}}




## Seurat (upper cutoff)
{{% button href="https://github.com/jfear/larval_gonad/blob/10050d3a5720091778329e72f3734a4fba86625e/scrnaseq-wf/scripts/scrnaseq_combine_force.Rmd#L60-L64" %}}Code{{% /button %}}


## Monocle ()
{{% button href="" %}}Code{{% /button %}}

