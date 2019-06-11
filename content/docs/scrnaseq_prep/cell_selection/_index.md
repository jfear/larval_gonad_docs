---
title: "Cell Selection"
weight: 1
# bookFlatSection: false
# bookShowToC: true
---
# Cell Selection

## TL;DR

**Problem:** Identify individual cells from scRNA-Seq data.

To identify non-empty GEMs, we looked at 3 different runs of `cell ranger` and a third party tool `dropletUtils`:

* Cell ranger v2 with default parameters appears under performs returning ~500 cells per replicate.
* Cell ranger v2 with the `--force-cells` option forces the returning of the expected number of cells (~50% of cells loaded) but is *ad hoc*.
* Cell ranger v3 updated the cell calling algorithm to perform better with low RNA contend cells. This method performs most similarly to the cell ranger v2 `--force-cells`.
* The third party method `dropletUtils`, appears to over call cells.

Without having ground truth it is difficult to know which method performs the best.
In the end, consistency is more important than accuracy, empty GEMs will likely cluster together or be spread across clusters having little influence on overall results. 
A consensus method is likely to be the most conservative solution.
I will exclude Cell ranger v2 default parameter from the consensus because the consensus will always equal cell ranger v2 with defaults calls because we are taking the intersection.
The 3-way consensus gives the following cell number per sample:

* Testis Replicate 1: 2,717
* Testis Replicate 2: 2,936
* Testis Replicate 3: 7,245
* Testis Replicate 4: 3,000 
<span style="font-size: .8em; color: #ff0000">@Sharvani, how many cells were loaded in replicate 4? I may be using the wrong force setting.</span>

I also ran a doublet detection software and found a modest amount of doublets that should be removed before clustering:

* Testis Replicate 1: 48
* Testis Replicate 2: 45
* Testis Replicate 3: 169
* Testis Replicate 4: 60 

**I propose to move forward using these cell calls and removing doublets for the next step.**

## Overview

{{<figure src="https://cdn.technologynetworks.com/tn/images/thumbs/webp/640_360/10x-genomics-extends-their-application-portfolio-305346.webp?v=9720678"
caption="**Figure 1. The 10X Library Preparation consists of mixing individual cells with individual gel beads (GEM).**" width="80%">}}

10X genomics is a droplet based method where cells are mixed with gel beads that contain indices and all materials needed for the first strand cDNA synthesis (**Figure 1**).
The 10X genomics kit that we are using has 737,280 GEMs for each sample.
Each GEM is coated with oligonucleotides that contain two indices: 
The first index is unique for each GEM and allows for demultiplexing of cells.
The second index is unique for each captured mRNA molecule and allows for counting unique molecules (UMI) instead of reads.
Cells are loaded with GEMs using a Poisson distribution which results in most GEMs being empty.
The first processing step is to identify which GEMs contain an individual cell.
This be broken down into two steps: remove empty GEMs and remove GEMs with two or more cells.

To identify empty GEMs we need to compare the RNA content among GEMs and identify a threshold that separate empty GEMs.
This is complicated by the fact that there is ambient RNA in the buffer that gives empty GEMs a signal that can be similar to a low RNA content cell.
10X Genomics provides their own software for calling cells ([`cell ranger`](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/algorithms/overview)), however results vary between versions and parameters.
According to the 10X Genomics website, `v3` has improved calling of low RNA content cells.
Both versions identify cells with high RNA content by calculating a total unique molecular index (UMI) cutoff based on distributional assumptions.
Version 3 of the software adds an additional step of comparing the remaining putative "empty" GEMs to each other.
Based on this comparison they can separate low RNA content cells from empty GEMs.

According to 10X Genomics' website, they expect a capture rate of ~50% of loaded cells.
**Table 1** shows our expectations as well as the number of reads we sequenced for each replicate.

**Table 1. Expectations for cell capture and read counts.**

| Rep      | Approx Num Cells Loaded | Expected Num Cells Captured | Num of Reads |
|----------|-------------------------|-----------------------------|--------------|
| Testis 1 | 6,000                   | 3,000                       | 119,932,060  |
| Testis 2 | 6,000                   | 3,000                       | 148,921,414  |
| Testis 3 | 16,000                  | 8,000                       | 131,590,010  |
| Testis 4 | 6,000                   | 3,000                       | 148,921,414  |


The identification of GEMs with 2 or more cells can also be challenging.
There are two types of multiplets that can occur: 

* Homeotypic: 2 or more cells from the same cell type
* Heterotypic: 2 or more cells from different cell types.

Homeotypic multiplets are is impossible to identify bioinformatically.
Some methods set a high end UMI threshold to remove putative homeotypic multiplets, but I am hesitant to do this in our data because our cell types have an expected large dynamic range of RNA content.
In contrast, heterotypic multiplets can be identified by modeling *in silico* doublets by mixing two or more cells from different cell type clusters.

Sharvani visualized single-cell preps after filtration (35 μm), and she dose see evidence of multiple small cells stuck together.
Because we are using a filter we are biasing our chance of capturing large cell aggregates. 
In particular, fused later staged primary spermatocytes are too large (~50 μm) to pass through the filter.
While multiplets from smaller somatic cells, spermatogonia, or early stage primary spermatocytes may pass through the filter.
Sharvani is working on getting an estimate of the frequency of multiplets after filtration. 

## Results 

### Empty Cell Identification

I display results for each replicate in the tabs below.
I have full descriptions of each section on the **Testis 1** tab.

{{< tabs "Cell Selection By Sample" >}}
{{< tab "Testis 1" >}}
#### `cellranger-wf` (Cell Ranger v2.1.1; defaults)

When we started this project `cell ranger` v2.1.1 was available.
This section shows results from running this program with default parameters.

{{<figure src="cellranger2_testis1_summary.png" width="100%">}}

This is the summary figure provided by `cell ranger`.
In it we see various summary statistics along with the Barcode Rank plot.
Things to pay attention to:

* The three measure in green at the top (*Estimated Number of Cells*, *Mean Reads per Cell*, and *Median Genes per Cell*). 
We are looking for a balance between these numbers.
In general, cells are added following the distribution in the barcode rank plot. 
In other words, cells with fewer reads are added as we increase the *Estimated Number of Cells*.

* The *Fraction of Reads in Cells* (2nd number below plot). 
We expect that most reads should be in a cell and not in empty GEMs.

* The percent of *Reads Mapped to Genome*.
This number will be the same across cell ranger runs, but indicates how good our samples are. 
**Testis 1** has the lowest mapping rate of 73%.
This is considered moderate to low quality in Bulk RNA-Seq studies where we typically aim for >90% mapping.

* The Barcode Rank Plot.
This plot shows the Log Total UMI Count (Y-axis) vs the Log Rank Ordered Cell Count (X-axis).
Light gray points are empty GEMs and colored points are considered non-empty cells.
The goal of cell selection is to find the cutoff along this curve.

#### `cellranger-force-wf` (Cell Ranger v2.1.1; force)

I was surprised that `cell ranger` with defaults was only giving ~500 cells per sample.
The 10X website points out that the v2 algorithm does not perform well on cell types with a high dynamic range and cell types with low RNA content.
They suggested using the `--force-cells` option which allows you to set how many cells you get back.
10X also suggested that the expected capture rate is ~50% of the number of loaded cells.
I used this estimate (**Table 1**) to re-run `cell ranger` with the forced setting.
This gives us many more reads than the defaults, but at the cost of *Median Genes per Cell*.
However, the *Fraction Reads in Cells* is much closer to the >70% suggested by 10X.

{{<figure src="cellranger2_testis1_force_summary.png" width="100%">}}

#### `cellranger3-wf (Cell Ranger v3.0.1; defaults)

When asked to revisit these decision points, I looked back at the 10X website to find `cell ranger` v3 was released.
Version 3 boasts an improved cell selection algorithm that handles low RNA content cells better than v2. 
This section has results from v3.0.1 using default settings. 
These results tend to be very close to v2 with the forced setting.

{{<figure src="cellranger3_testis1_summary.png" width="100%">}}

#### `droputils` (DropletUtils v1.2.1)

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

#### Pairwise Similarity (Jaccard)

In this section I look at the similarity of calls between the different methods.
To do this I use the Jaccard index which compares to boolean arrays.
The `cellranger-foce-wf` and the `cellranger3-wf` always have the highest similarity.
The `droputils` and the `cellranger-wf` always have the lowest similarity.


| method              |   cellranger-wf |   cellranger-force-wf |   cellranger3-wf |   droputils |
|---------------------|-----------------|-----------------------|------------------|-------------|
| cellranger-wf       |          1      |                0.8219 |           0.8342 |      0.0537 |
| cellranger-force-wf |          0.8219 |                1      |           0.9771 |      0.1919 |
| cellranger3-wf      |          0.8342 |                0.9771 |           1      |      0.2146 |
| droputils           |          0.0537 |                0.1919 |           0.2146 |      1      |

#### Consensus

As previously discussed, I think a consensus method is probably the conservative. 
In this section I summarize different ways of taking the consensus. 
I removed `cellranger-wf` from the consensus measure, because it will give the same results as just running `cellranger-wf`.
I provide a summary for the 3-way consensus (`cellranger-force-wf`, `cellranger3-wf`, `droputils`) and the 2-way consensus (`cellranger3-wf` and `droputils`).

* Number of cells with 3-way consensus:  2,717
* Number of cells with 2-way consensus:  2,790
* Number of different calls between consensus with 3 vs 2 measures:  73
* Jaccard similarity of consensus with 3 vs 2 measures:  0.9948479074034865

{{<figure src="testis1_combined_upset.svg" width="100%">}}

This is an UpSet plot. 
It is like a high dimensional Venn Diagram.
Each bar represents the number of cells for a given set.
The set is represented below as black dots.
For example the first bar is the 4-way consensus (intersection of all 4 methods).
This plot clearly shows that `droputils` calls way more cells than other methods (here 11,094). 
Also that the 4-way consensus is equivalent to `cellranger-wf`. 

<!-- {{% button href="https://github.com/jfear/larval_gonad/blob/5f8f4569ea253ab96d497055e7ae6ebb4c82a744/scrnaseq-wf/Snakefile#L96-L121" %}}Code{{% /button %}} -->
{{< /tab >}}


{{< tab "Testis 2" >}}
#### `cellranger-wf` (Cell Ranger v2.1.1; defaults)
{{<figure src="cellranger2_testis2_summary.png" width="100%">}}

#### `cellranger-force-wf` (Cell Ranger v2.1.1; force)
{{<figure src="cellranger2_testis2_force_summary.png" width="100%">}}

#### `cellranger3-wf` (Cell Ranger v3.0.1; defaults)
{{<figure src="cellranger3_testis2_summary.png" width="100%">}}

#### `droputils` (DropletUtils v1.2.1)
{{<figure src="dropletutils_testis2_bc_rank.png" width="70%">}}

#### Pairwise Similarity (Jaccard)
| method              |   cellranger-wf |   cellranger-force-wf |   cellranger3-wf |   droputils |
|---------------------|-----------------|-----------------------|------------------|-------------|
| cellranger-wf       |          1      |                0.6678 |           0.2083 |      0.2933 |
| cellranger-force-wf |          0.6678 |                1      |           0.5335 |      0.6081 |
| cellranger3-wf      |          0.2083 |                0.5335 |           1      |      0.6539 |
| droputils           |          0.2933 |                0.6081 |           0.6539 |      1      |


#### Consensus
* Number of cells with 3-way consensus:  2,936
* Number of cells with 2-way consensus:  4,801
* Number of different calls between consensus with 3 vs 2 measures:  1,865
* Jaccard similarity of consensus with 3 vs 2 measures:  0.7473584394473043

{{<figure src="testis2_combined_upset.svg" width="100%">}}

<!-- {{% button href="https://github.com/jfear/larval_gonad/blob/5f8f4569ea253ab96d497055e7ae6ebb4c82a744/scrnaseq-wf/Snakefile#L96-L121" %}}Code{{% /button %}} -->
{{< /tab >}}


{{< tab "Testis 3" >}}
#### `cellranger-wf` (Cell Ranger v2.1.1; defaults)
{{<figure src="cellranger2_testis3_summary.png" width="100%">}}

#### `cellranger-force-wf` (Cell Ranger v2.1.1; force)
{{<figure src="cellranger2_testis3_force_summary.png" width="100%">}}

#### `cellranger3-wf` (Cell Ranger v3.0.1; defaults)
{{<figure src="cellranger3_testis3_summary.png" width="100%">}}

#### `droputils` (DropletUtils v1.2.1)
{{<figure src="dropletutils_testis3_bc_rank.png" width="70%">}}

#### Pairwise Similarity (Jaccard)
| method              |   cellranger-wf |   cellranger-force-wf |   cellranger3-wf |   droputils |
|---------------------|-----------------|-----------------------|------------------|-------------|
| cellranger-wf       |          1      |                0.81   |           0.6968 |      0.0107 |
| cellranger-force-wf |          0.81   |                1      |           0.8489 |      0.2006 |
| cellranger3-wf      |          0.6968 |                0.8489 |           1      |      0.3139 |
| droputils           |          0.0107 |                0.2006 |           0.3139 |      1      |

#### Consensus
* Number of cells with 3-way consensus:  7,245
* Number of cells with 2-way consensus:  12,515
* Number of different calls between consensus with 3 vs 2 measures:  5,270
* Jaccard similarity of consensus with 3 vs 2 measures:  0.8678270465489567

{{<figure src="testis3_combined_upset.svg" width="100%">}}

<!-- {{% button href="https://github.com/jfear/larval_gonad/blob/5f8f4569ea253ab96d497055e7ae6ebb4c82a744/scrnaseq-wf/Snakefile#L96-L121" %}}Code{{% /button %}} -->
{{< /tab >}}


{{< tab "Testis 4" >}}

#### `cellranger-wf` (Cell Ranger v2.1.1; defaults)
{{<figure src="cellranger2_testis4_summary.png" width="100%">}}

#### `cellranger-force-wf` (Cell Ranger v2.1.1; force)
{{<figure src="cellranger2_testis4_force_summary.png" width="100%">}}

#### `cellranger3-wf` (Cell Ranger v3.0.1; defaults)
{{<figure src="cellranger3_testis4_summary.png" width="100%">}}

#### `droputils` (DropletUtils v1.2.1)
{{<figure src="dropletutils_testis4_bc_rank.png" width="70%">}}

#### Pairwise Similarity (Jaccard)
| method              |   cellranger-wf |   cellranger-force-wf |   cellranger3-wf |   droputils |
|---------------------|-----------------|-----------------------|------------------|-------------|
| cellranger-wf       |          1      |                0.9237 |           0.5776 |      0.01   |
| cellranger-force-wf |          0.9237 |                1      |           0.6538 |      0.0863 |
| cellranger3-wf      |          0.5776 |                0.6538 |           1      |      0.4325 |
| droputils           |          0.01   |                0.0863 |           0.4325 |      1      |

#### Consensus
* Number of cells with 3-way consensus:  3,000
* Number of cells with 2-way consensus:  15,033
* Number of different calls between consensus with 3 vs 2 measures:  12,033
* Jaccard similarity of consensus with 3 vs 2 measures:  0.6538361957366013

{{<figure src="testis3_combined_upset.svg" width="100%">}}

<!-- {{% button href="https://github.com/jfear/larval_gonad/blob/5f8f4569ea253ab96d497055e7ae6ebb4c82a744/scrnaseq-wf/Snakefile#L96-L121" %}}Code{{% /button %}} -->
{{< /tab >}}
{{< /tabs >}}


### Doublet Detection

Next I wanted to identify cells that are potential heterotypic doublets. 
There are several software packages available, but all use essentially the same algorithm.

1. Cells are clustered into individual cells types
2. Cells from different clusters are combined *in silico* to form synthetic doublets.
3. Cells + synthetic doublets are mixed and re-clustered.
4. Cells that cluster with the synthetic doublets are thought to be heterotypic doublets.

I am not sure how well this works with cell lineages. 
For example, the transition from spermatogonia to spermatocyte will probably have an intermediate cell type that has both spermatogonia specific transcripts and spermatocyte specific transcripts.
If this intermediate cell type exists it may be called as a heterotypic doublet.
Also this method is dependent on how the clustering is done. 
Here I use sane parameter and do not do any kind of optimization.
Again outputs are displayed below in different tabs for different samples.
On the **Testis 1** I added additional text describing what results are provided.

I included cells from the 3-way consensus above.

{{< tabs "Doublet Detection By Sample" >}}
{{< tab "Testis 1" >}}
#### Simulation Cutoff

The tool I am using is called `scrublet`. 
It provides this diagnostic histogram to help identify the threshold for what is called a doublet.
In the right panel are the simulated doublets. 
It is expected to be a biomodal distribution with doublets being on the right and singlets (or homeotypic doublets) being on the left.
Our data does not proved a super clear modality, this is probably due to the fact we have two lineage and a large dynamic range of RNA content.
I set the threshold (t = 0.25) to be the same for all replicates. 

{{<figure src="testis1_scrublet_histogram.png" width="100%">}}


#### Projects of Doublets (n = 48)

This plot shows the UMAP projects of the cells with the doublet calls.
UMAP is a manifold method similar to tSNE, but does a better job maintaining global structure.
In **Testis 1** we can see these intermediate cell types being labeled as doublets.
This is not true for the other replicates.

{{<figure src="testis1_scrublet_umap.png" width="100%">}}
{{< /tab >}}


{{< tab "Testis 2" >}}
#### Simulation Cutoff
{{<figure src="testis2_scrublet_histogram.png" width="100%">}}

#### Projects of Doublets (n = 45)
{{<figure src="testis2_scrublet_umap.png" width="100%">}}
{{< /tab >}}


{{< tab "Testis 3" >}}
#### Simulation Cutoff
{{<figure src="testis3_scrublet_histogram.png" width="100%">}}

#### Projects of Doublets (n = 169)
{{<figure src="testis3_scrublet_umap.png" width="100%">}}
{{< /tab >}}


{{< tab "Testis 4" >}}
#### Simulation Cutoff
{{<figure src="testis4_scrublet_histogram.png" width="100%">}}

#### Projects of Doublets (n = 60)
{{<figure src="testis4_scrublet_umap.png" width="100%">}}
{{< /tab >}}
{{< /tabs >}}


## Conclusions

### Data Quality

The overall data quality is moderate to good. 
We map 73-92% of reads to the genome.
I have not explicitly looked at rRNA content or other QC metics. 

### Cell Selection

I employed 4 different methods for cell selection.
I think `cell ranger` v2 with default parameters under calls cells and `dropletUtils` over calls cells.
Using `cell ranger` v2 with `--force-cells` and `cell ranger` v3 gives very similar results. 
Using a 3-way consensus (`cellranger-force-wf`, `cellranger3-wf`, `droputils`) we get the following cell calls.

* Testis Replicate 1: 2,717
* Testis Replicate 2: 2,936
* Testis Replicate 3: 7,245
* Testis Replicate 4: 3,000 

### Doublet Detection

I used one program for doublet detection `scrublet`, but it is only able to identify heterotypic doublets.
There is evidence of heterotypic doublets, but the total number is relatively small.

* Testis Replicate 1: 48
* Testis Replicate 2: 45
* Testis Replicate 3: 169
* Testis Replicate 4: 60 
