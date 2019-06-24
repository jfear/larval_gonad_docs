---
title: "Cell Selection"
weight: 1
# bookFlatSection: false
# bookShowToC: true
---

## Cell Selection

### TL;DR

**Problem:** Identify individual cells from scRNA-Seq data.

The first step in scRNA-Seq analysis is to identify which reads come from individual cells.
This requires removing empty GEMs (**Figure 1**), heterotypic and homotypic multiplets, and cells with low expression diversity.
I selected the following as putative cells for clustering and downstream analysis (**Table 1**).
Replicates 3 and 4 are puzzling, with having more captured cells than expected.
Either I am over-calling cells or more cells were loaded than thought.

**Table 1. Final cell selection counts.**

| Replicate | Approx. Num. Cells Loaded | Expected Num. Cells Captured | Num. of Cells Selected | Num. of Reads |
|-----------|---------------------------|------------------------------|------------------------|---------------|
| Testis 1  | 6,000                     | 3,000                        | 2,710                  | 119,932,060   |
| Testis 2  | 6,000                     | 3,000                        | 4,226                  | 148,921,414   |
| Testis 3  | 12,000                    | 8,000                        | 11,946                 | 131,590,010   |
| Testis 4  | 12,000                    | 8,000                        | 14,697                 | 159,678,110   |

### Overview

{{<figure src="https://cdn.technologynetworks.com/tn/images/thumbs/webp/640_360/10x-genomics-extends-their-application-portfolio-305346.webp?v=9720678"
caption="**Figure 1. The 10X Library Preparation consists of mixing cells with gel beads (GEMs).** The goal of this process is to get a single cell associated with a single GEM." width="80%">}}

10X genomics is a droplet based method where cells are mixed with gel beads (GEMs) that contain indices and all materials needed for the first strand cDNA synthesis (**Figure 1**).
Cells are loaded with GEMs using a Poisson distribution which results in most GEMs being empty.
The 10X genomics kit that we are using has 737,280 GEMs per sample.
Oligonucleotides attached to each GEM contain a cell index and a molecule index.
I want to only use GEMs capturing a single cell, so I need to remove empty GEMs and GEMs that captured more than 1 cell.

We can model total unique molecule expression (UMI) to separate GEMs that contain ≥1 cell from empty GEMs.
This will account for any ambient RNA in the media.
10X Genomics provides their own software for calling cells ([`cell ranger`](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/algorithms/overview)).
Recently, 10X genomics released version 3 of `cell ranger`, which boasts improved cell calling for low RNA content cells.
Here I evaluate `cell ranger` `v2` and `v3` as well as a 3rd party tool `DropLetUtils`.

In addition to removing GEMs with 0 cells, we also want to remove GEMs with ≥2 cells (a.k.a multiplets).
There are two types of multiplets that can occur:

* Heterotypic: ≥2 cells from different cell types.
* Homeotypic: ≥2 cells from the same cell type

Heterotypic multiplets can be identified *in silico*.
Essentially I create synthetic doublets by mixing two or more cells from different cell type clusters.
I then re-cluster cells + synthetic cells.
Cells that cluster with synthetic doublets are likely to be heterotypic multiplets and are removed. 
There are several tools available to do this analysis, I decided to use `scrublet` because it works with raw 10X data.

We do expect biases when it comes to the types of multiplets that we capture. 
We do not expect to capture doublets of large cells or large aggregates because there is a filtration step (35 μm) during sample prep.
In particular, fused later staged primary spermatocytes are too large (~50 μm) to pass through the filter.
While multiplets from smaller somatic cells, spermatogonia, or early stage primary spermatocytes may easily pass through the filter.
Sharvani does see evidence for multiple small cells stuck together after filtration.
She is working on getting an estimate of the frequency of multiplets to give better insight of how big of a problem this may be.

### Results

**NOTE:** Below I display results for each replicate on different tabs. 
Click the replicate name to view results. 
Complete descriptions of the data are included with **Testis 1**.

#### Empty GEM Identification

To identify empty GEMs I used multiple software and parameterizations.

**`cellranger-wf`** uses cell ranger v2 with default settings. 
This workflow calls the fewest number of cells (~500 cells / rep).
According to 10X Genomics we expect to capture ~50% of loaded cells, but this method gives ≤8%.
This method is known not to perform well when there is a large dynamic range of RNA content.
Our samples are from the *Drosophila* testis which is known to contain nearly quiescent cells and extremely high RNA content cells.
Therefore, we think this method is under calling captured cells.

**`cellranger-force-wf`** uses cell ranger v2 with the `--force-cells` option.
When samples contain a high dynamic range of RNA content, 10X Genomics suggests to use the `--force-cells`.
Essentially this method orders cells according to total UMI content and then keeps the highest `N` cells. 
We specified our forced settings to capture 50% of loaded cells, but this is *ad hoc* and likely over calls captured cells. 

**`cellranger3-wf`** uses cell ranger v3 with default settings.
Recently, 10X Genomics released version 3 of cell ranger. 
This version adds an additional step to the cell calling algorithm which improves calling of low RNA content cells.
Interestingly, this method called more cells than `cellranger-force-wf` in all replicates expect **Testis 1**. 

**`droputils`** uses DropLetUtils v3.0.1 with default settings.
DropLetUtils is an R packaged with several algorithms for scRNA-Seq.
I used the `emptydrops` function which models UMI to classify empty GEMs.
This method performs very different for each replicate. 
This method seems to over call cells in **Testis 1**, **Testis 3**, and **Testis 4** while performing similarly to `cell ranger` on **Testis 2**.

Without a ground truth data set it is impossible to know which method is best approximating real cell calls.
Consensus is often used in the absence of truth. 
However, sense all of these methods use total UMI in their models, the 4-way consensus would be the same as the most conservative cell calls (`cellranger-wf`).
Given the known limitation of `cellranger-wf` and the *ad hoc* nature of `cellranger-force-wf` I decided to use the consensus of `cellranger3-wf` and `droputils`. 


**Table 2. Cell count after removing empty GEMs.**

| Replicate | `cellranger-wf` | `cellranger-force-wf` | `cellranger3-wf` | `droputils` | 2-way Consensus<sup>‡</sup> |
|-----------|-----------------|-----------------------|------------------|-------------|-----------------------------|
| Testis 1  | 483             | 3,000                 | 2,826            | 13,884      | 2,790                       |
| Testis 2  | 550             | 3,000                 | 6,385            | 5,765       | 4,801                       |
| Testis 3  | 423             | 8,000                 | 12,485           | 39,872      | 12,515                      |
| Testis 4  | 349             | 8,000                 | 15,033           | 34,761      | 15,033                      |

<sup>‡</sup>Intersection of `cellranger3-wf` and `droputils`.


{{< tabs "Cell Selection By Sample" >}}
{{< tab "Testis 1" >}}

##### `cellranger-wf` (v2.1.1; defaults)

{{<figure src="cellranger2_testis1_summary.png" width="100%">}}

Summary statistics and barcode rank plot provided by `cell ranger`.
Things to pay attention to:

* The three numbers in green at the top (*Estimated Number of Cells*, *Mean Reads per Cell*, and *Median Genes per Cell*).
We are looking for a balance between these numbers.
As cells with lower UMI are added, the *Mean Reads per Cell* and *Median Genes per Cell* will decrease.
We do not want these numbers to get too low as the added cells will not have enough signal.

* The *Fraction of Reads in Cells* (2nd number below plot).
We expect that most reads should be in a cell and not in empty GEMs.

* The percent of *Reads Mapped to Genome*.
This number will be the same across cell ranger runs, but indicates how good our samples are.
**Testis 1** has the lowest mapping rate of 73%.
This is considered moderate to low quality in Bulk RNA-Seq studies where we typically aim for ≥90% mapping.

* The Barcode Rank Plot.
This plot shows the Log Total UMI Count (Y-axis) vs the Log Rank Ordered Cell Count (X-axis).
Light gray points are empty GEMs and colored points are considered non-empty cells.
The goal of cell selection is to find the cutoff along this curve.

##### `cellranger-force-wf` (v2.1.1; force)

Note the *Fraction Reads in Cells* is closer to the >70%.

{{<figure src="cellranger2_testis1_force_summary.png" width="100%">}}

##### `cellranger3-wf (v3.0.1; defaults)

These results are very similar to `cellranger-force-wf`.

{{<figure src="cellranger3_testis1_summary.png" width="100%">}}

##### `droputils` (DropletUtils v1.2.1)

Finally, I wanted to use an external tool for cell selection.
The most popular tool I founds was an R package called `dropletUtils`.

>Lun ATL, Riesenfeld S, Andrews T, Dao T, Gomes T, participants in the 1st
Human Cell Atlas Jamboree, Marioni JC (2019). “EmptyDrops: distinguishing
cells from empty droplets in droplet-based single-cell RNA sequencing data.”
Genome Biol., 20, 63. doi: 10.1186/s13059-019-1662-y.

This tool also builds a classification model based on UMI content.
I do not have all of the summary stats for these cell calls, but I proved the barcode rank plot.
The tool estimates the knee (blue line) and the inflection point (green line) automatically.
Putative empty GEMs are in light gray and non-empty cells are in black.
This method always calls way more cells than `cell ranger` methods.

{{<figure src="dropletutils_testis1_bc_rank.png" width="70%">}}

##### Pairwise Similarity (Jaccard)

In this section I look at the similarity of calls between the different methods using the pairwise Jacaard similarity score.
The `cellranger-foce-wf` and the `cellranger3-wf` always have the highest similarity.
The `droputils` and the `cellranger-wf` always have the lowest similarity.

| method              |   cellranger-wf |   cellranger-force-wf |   cellranger3-wf |   droputils |
|---------------------|-----------------|-----------------------|------------------|-------------|
| cellranger-wf       |          1      |                0.8219 |           0.8342 |      0.0537 |
| cellranger-force-wf |          0.8219 |                1      |           0.9771 |      0.1919 |
| cellranger3-wf      |          0.8342 |                0.9771 |           1      |      0.2146 |
| droputils           |          0.0537 |                0.1919 |           0.2146 |      1      |

##### Consensus

I provide a summary for the 3-way consensus (`cellranger-force-wf`, `cellranger3-wf`, `droputils`) and the 2-way consensus (`cellranger3-wf` and `droputils`).

* Number of cells with 3-way consensus:  2,717
* Number of cells with 2-way consensus:  2,790
* Number of different calls between consensus with 3 vs 2 measures:  73
* Jaccard similarity of consensus with 3 vs 2 measures:  0.9948479074034865

{{<figure src="testis1_combined_upset.svg" width="100%">}}

This is an UpSet plot, it is like a high dimensional Venn Diagram.
Each bar represents the number of cells for a given set.
The set is represented below as black dots.
For example the first bar is the 4-way consensus (intersection of all 4 methods).

<!-- {{% button href="https://github.com/jfear/larval_gonad/blob/5f8f4569ea253ab96d497055e7ae6ebb4c82a744/scrnaseq-wf/Snakefile#L96-L121" %}}Code{{% /button %}} -->
{{< /tab >}}

{{< tab "Testis 2" >}}

##### `cellranger-wf` (v2.1.1; defaults)

{{<figure src="cellranger2_testis2_summary.png" width="100%">}}

##### `cellranger-force-wf` (v2.1.1; force)

{{<figure src="cellranger2_testis2_force_summary.png" width="100%">}}

##### `cellranger3-wf` (v3.0.1; defaults)

{{<figure src="cellranger3_testis2_summary.png" width="100%">}}

##### `droputils` (DropletUtils v1.2.1)

{{<figure src="dropletutils_testis2_bc_rank.png" width="70%">}}

##### Pairwise Similarity (Jaccard)

| method              |   cellranger-wf |   cellranger-force-wf |   cellranger3-wf |   droputils |
|---------------------|-----------------|-----------------------|------------------|-------------|
| cellranger-wf       |          1      |                0.6678 |           0.2083 |      0.2933 |
| cellranger-force-wf |          0.6678 |                1      |           0.5335 |      0.6081 |
| cellranger3-wf      |          0.2083 |                0.5335 |           1      |      0.6539 |
| droputils           |          0.2933 |                0.6081 |           0.6539 |      1      |

##### Consensus

* Number of cells with 3-way consensus:  2,936
* Number of cells with 2-way consensus:  4,801
* Number of different calls between consensus with 3 vs 2 measures:  1,865
* Jaccard similarity of consensus with 3 vs 2 measures:  0.7473584394473043

{{<figure src="testis2_combined_upset.svg" width="100%">}}

{{< /tab >}}

{{< tab "Testis 3" >}}

##### `cellranger-wf` (v2.1.1; defaults)

{{<figure src="cellranger2_testis3_summary.png" width="100%">}}

##### `cellranger-force-wf` (v2.1.1; force)

{{<figure src="cellranger2_testis3_force_summary.png" width="100%">}}

##### `cellranger3-wf` (v3.0.1; defaults)

{{<figure src="cellranger3_testis3_summary.png" width="100%">}}

##### `droputils` (DropletUtils v1.2.1)

{{<figure src="dropletutils_testis3_bc_rank.png" width="70%">}}

##### Pairwise Similarity (Jaccard)

| method              |   cellranger-wf |   cellranger-force-wf |   cellranger3-wf |   droputils |
|---------------------|-----------------|-----------------------|------------------|-------------|
| cellranger-wf       |          1      |                0.81   |           0.6968 |      0.0107 |
| cellranger-force-wf |          0.81   |                1      |           0.8489 |      0.2006 |
| cellranger3-wf      |          0.6968 |                0.8489 |           1      |      0.3139 |
| droputils           |          0.0107 |                0.2006 |           0.3139 |      1      |

##### Consensus

* Number of cells with 3-way consensus:  7,245
* Number of cells with 2-way consensus:  12,515
* Number of different calls between consensus with 3 vs 2 measures:  5,270
* Jaccard similarity of consensus with 3 vs 2 measures:  0.8678270465489567

{{<figure src="testis3_combined_upset.svg" width="100%">}}

{{< /tab >}}

{{< tab "Testis 4" >}}

##### `cellranger-wf` (v2.1.1; defaults)

{{<figure src="cellranger2_testis4_summary.png" width="100%">}}

##### `cellranger-force-wf` (v2.1.1; force)

<!-- TODO update running --force-cells 80000 -->
{{<figure src="cellranger2_testis4_force_summary.png" width="100%">}}

##### `cellranger3-wf` (v3.0.1; defaults)

{{<figure src="cellranger3_testis4_summary.png" width="100%">}}

##### `droputils` (DropletUtils v1.2.1)

{{<figure src="dropletutils_testis4_bc_rank.png" width="70%">}}

##### Pairwise Similarity (Jaccard)

| method              |   cellranger-wf |   cellranger-force-wf |   cellranger3-wf |   droputils |
|---------------------|-----------------|-----------------------|------------------|-------------|
| cellranger-wf       |          1      |                0.9237 |           0.5776 |      0.01   |
| cellranger-force-wf |          0.9237 |                1      |           0.6538 |      0.0863 |
| cellranger3-wf      |          0.5776 |                0.6538 |           1      |      0.4325 |
| droputils           |          0.01   |                0.0863 |           0.4325 |      1      |

<!-- TODO update running --force-cells 80000 -->

##### Consensus

* Number of cells with 3-way consensus:  3,000
* Number of cells with 2-way consensus:  15,033
* Number of different calls between consensus with 3 vs 2 measures:  12,033
* Jaccard similarity of consensus with 3 vs 2 measures:  0.6538361957366013

{{<figure src="testis4_combined_upset.svg" width="100%">}}

<!-- TODO update running --force-cells 80000 -->

{{< /tab >}}
{{< /tabs >}}

#### Heterotypic Doublet Detection

Next I wanted to identify cells that are heterotypic doublets.
There are several software packages available, but all use essentially the same algorithm.

1. Cells are clustered into individual cells types
2. Cells from different clusters are combined *in silico* to form synthetic doublets.
3. Cells + synthetic doublets are mixed and re-clustered.
4. Cells that cluster with the synthetic doublets are likely heterotypic multiplets.

Again outputs are displayed below in different tabs for different samples.
On the **Testis 1** I added additional text describing what results are provided.

I am using cells from the 2-way consensus above.

**Table 3. Summary of homotypic multiplets.**

| Replicate | Num Homotypic Multiplets |
|-----------|--------------------------|
| Testis 1  | 48                       |
| Testis 2  | 45                       |
| Testis 3  | 169                      |
| Testis 4  | 60                       |

{{< tabs "Doublet Detection By Sample" >}}
{{< tab "Testis 1" >}}

##### Simulation Cutoff

`scrublet` provides this diagnostic histogram to help identify the threshold for what is called a doublet.
In the right panel are the simulated doublets.
It is expected to be a bimodal distribution with doublets being on the right and singlets (or homeotypic doublets) being on the left.
Our data does not provide a clear modality, this is probably due to the fact we have two lineage and a large dynamic range of RNA content.
I set the threshold (t = 0.25) to be the same for all replicates.

{{<figure src="testis1_scrublet_histogram.png" width="100%">}}

##### Projects of Doublets (n = 48)

This plot shows the UMAP projection colored by doublet calls.
In **Testis 1** we can see these intermediate cell types being labeled as doublets.
This is not true for the other replicates.

{{<figure src="testis1_scrublet_umap.png" width="100%">}}

{{< /tab >}}

{{< tab "Testis 2" >}}

##### Simulation Cutoff

{{<figure src="testis2_scrublet_histogram.png" width="100%">}}

##### Projects of Doublets (n = 45)

{{<figure src="testis2_scrublet_umap.png" width="100%">}}

{{< /tab >}}

{{< tab "Testis 3" >}}

##### Simulation Cutoff

{{<figure src="testis3_scrublet_histogram.png" width="100%">}}

##### Projects of Doublets (n = 169)

{{<figure src="testis3_scrublet_umap.png" width="100%">}}

{{< /tab >}}

{{< tab "Testis 4" >}}

##### Simulation Cutoff

{{<figure src="testis4_scrublet_histogram.png" width="100%">}}

##### Projects of Doublets (n = 60)

{{<figure src="testis4_scrublet_umap.png" width="100%">}}

{{< /tab >}}
{{< /tabs >}}

#### Homotypic Doublet and Low Complexity Detection

Finally, I attempt to remove homotypic cells using a high expression cutoff.
At the same time I am checking if additional low expression cutoffs would be useful, and if I should remove cells with high mitochondrial or rRNA expression.
In the absence of truth, setting these thresholds is *ad hoc*. 
To get a better understanding of the value of these different criteria I use a grid search.
For each criteria (low gene cutoff, high gene cutoff, percent mitochondiral reads, percent rRNA reads) I selected 4 levels.
I ran Seurat (v2) clustering on the different combinations in these same (12 dimensions), aiming to get 12-13 clusters. 
I used the Adjusted Rand Index to look at similarity of cluster calls for each pairwise comparison.

In general, filtering by the percent mitochondrial or percent ribosomal had no affect on clustering. 
Low end cutoffs of 200 or 500 expressed genes behaved very similarly, while increasing this to 1,000 gene changes clustering outcome.
In **Testis 1** there was little difference using a high end cutoff. 
However, in other samples any high cutoff created different clusters than using no high end cutoff.
I am interpreting this as the most extreme high expressing cells can affect clustering. 

##### Cutoff Criteria

* **NO** Mitochondrial criteria
* **NO** rRNA criteria
* Remove cells **≤200 expressed genes**.
* Remove cells **≥5,000 expressed genes**.

{{< tabs "Gene Level Filters" >}}
{{< tab "Testis 1" >}}

##### Distribution of cells based on gene level counts

{{<figure src="./testis1_vln.png" width="80%">}}

Cell distributions for:

* nGene: The total number genes expressed (>0). Cells at the high end of this distribution are possibly homotypic doublets.
* nUMI: The total UMI count per cell. Cells at the high end of this distribution are possibly homotypic doublets.
* percent.mito: The percent of reads mapping to the mitochondria. Cells at the high end of this distribution are possibly dying.
* percent.ribo: The percent reads mapping to the rRNA. Cells at the high end of this distribution possibly had poor poly(A) selection.

##### Relationship of UMI vs other measures

{{<figure src="./testis1_scatter.png" width="80%">}}

As expeted the nGene and nUMI are highly correlated. 
However, cells high in percent mitochondrial or ribosomal reads tend to be in the low to middle of the UMI distribution.

##### Grid search summary

{{<figure src="./testis1_heatmap_grid.svg" link="./testis1_heatmap_grid.svg" target="_blank" width="100%" caption="click image to enlarge">}}

The similarity matrix of [Adjusted Rand Indexes](https://scikit-learn.org/stable/modules/clustering.html#adjusted-rand-score). 
A score of 1 indicates cluster calls were identical. 

{{< /tab >}}

{{< tab "Testis 2" >}}

##### Distribution of cells based on gene level counts

{{<figure src="./testis2_vln.png" width="80%">}}

##### Relationship of UMI vs other measures

{{<figure src="./testis2_scatter.png" width="80%">}}

##### Grid search summary

{{<figure src="./testis2_heatmap_grid.svg" link="./testis2_heatmap_grid.svg" target="_blank" width="100%" caption="click image to enlarge">}}

{{< /tab >}}

{{< tab "Testis 3" >}}

##### Distribution of cells based on gene level counts

{{<figure src="./testis3_vln.png" width="80%">}}

##### Relationship of UMI vs other measures

{{<figure src="./testis3_scatter.png" width="80%">}}

##### Grid search summary

{{<figure src="./testis3_heatmap_grid.svg" link="./testis3_heatmap_grid.svg" target="_blank" width="100%" caption="click image to enlarge">}}

{{< /tab >}}

{{< tab "Testis 4" >}}

##### Distribution of cells based on gene level counts

{{<figure src="./testis4_vln.png" width="80%">}}

##### Relationship of UMI vs other measures

{{<figure src="./testis4_scatter.png" width="80%">}}

##### Grid search summary

{{<figure src="./testis4_heatmap_grid.svg" link="./testis4_heatmap_grid.svg" target="_blank" width="100%" caption="click image to enlarge">}}

{{< /tab >}}
{{< /tabs >}}

### Conclusions

In summary I applied the following approaches:

* **Removed Empty GEMs:** Using a consensus approach with `cellranger3-wf` and `DropLetUtils`
* **Removed heterotypic multiplets:** Using an *in silico* approach with `scrublet`.
* **Removed homotypic multiplets and low complexity cells:** Using a sane cutoff of (200≤ Num Expressed Genes per Cell <5,000) that does not change clustering too compared to similar criteria.

For the final number of selected cells see **Table 1**.