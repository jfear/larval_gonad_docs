---
title: "Cell Selection"
weight: 1
# bookFlatSection: false
# bookShowToC: true
---
# Cell Selection

## TL;DR

**Problem:** Identify individual cells from scRNA-Seq data.

Cell selection is variable with different parameters and versions.
Cell ranger v2 with default parameters under performs returning ~500 cells per replicate.
Cell ranger v2 with the `--force-cells` option forces the returning of the expected number of cells for each replicate, but this is rather *ad hoc*.
Cell ranger v3 with default options returns the expected number of cells or more. 
This seems less *ad hoc* than using the `--force-cells` option, but maybe over calling the number of cells especially in replicates 2 and 3.
Also the addition of low count cells seems to muddle cluster calling in downstream analysis. 
While similar clusters are identified, irrespective of this step, clusters called from cell ranger v2 `--force-cells` have better separation and representation of known markers from the literature.

## Overview

{{<figure src="https://cdn.technologynetworks.com/tn/images/thumbs/webp/640_360/10x-genomics-extends-their-application-portfolio-305346.webp?v=9720678"
caption="**Figure 1. The 10X Library Preparation consists of mixing individual cells with individual gel beads.**" width="80%">}}

After sequencing, the first step in scRNA-Seq analysis is to identify individual cells.
On the 10X Genomics platform, libraries are prepared by combining individual cells with individual beads (**Figure 1.**).
Each bead contains a unique barcode which is used for demultiplexing cells.
The goal of cell selection is to find beads that have 1 and only 1 cell.
The majority of beads will be empty because cells are loaded using a double Poisson distribution.
However, empty cells may still have signal due to RNA in the media.
In theoretically rare cases a bead may be combined with 2 or more cells.
This could be caused by incomplete dissociation or loading of 2 separate cells in the same bead.
This is particularly problematic when the 2+ cells are all from the same cell-type.

We visualized our single-cell preps after filtration (35 μm).
We do see some evidence of multiple small cells stuck together.
Because we are using a filter we are biasing our chance of capturing large cell aggregates. 
In particular, fused later staged primary spermatocytes are too large (~50 μm) to pass through the filter.
Any single cell-type multiplet will be from the smaller somatic cells, spermatogonia, or early stage primary spermatocytes.


To demultiplex and identify individual cells 10X Genomics provides their own software ([`cell ranger`](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/algorithms/overview)).
We tried `cell ranger` versions `v2` and `v3`.
According to the 10X Genomics website, `v3` has improved calling low RNA content cells.
Both versions begin by calculating a total unique molecular index (UMI) cutoff based on distributional assumptions.
This step is good at identifying cells with high RNA content.
It performs poorly when multiple cell-types are present with a high degree of RNA content heterogeneity. 
Our data contains a wide range of cell-types from high RNA-content primary spermatocytes to essentially quiescent stem cell populations.
Version 3 of the software adds an additional step of comparing the remaining putative "empty" cells to each other.
Based on this comparison they can separate low RNA content cells from empty GEMS.

After cell selection `cell ranger` aligns reads using `STAR` and provides the gene count matrix for downstream analysis.
It also provides summary statistics shown below.
According to 10X Genomics' website, they expect a capture rate of ~50% of loaded cells.
**Table 1** shows our expectations as well as the number of reads sequenced for each replicate.

**Table 1. Expectations for cell capture and read counts.**

| Rep      | Approx Num Cells Loaded | Expected Num Cells Captured | Num of Reads |
|----------|-------------------------|-----------------------------|--------------|
| Testis 1 | 6,000                   | 3,000                       | 119,932,060  |
| Testis 2 | 6,000                   | 3,000                       | 148,921,414  |
| Testis 3 | 16,000                  | 8,000                       | 131,590,010  |

## Results 

### Cell Ranger

Cell ranger summaries are displayed below (**click on tabs to view**). 

#### V2 Defaults

Cell ranger identified ~500 cells per replicate.
This is less than 10% of loaded cells being recovered.
Looking at the barcode rank plots we see that cells are called slightly past the first inflection point. 
We can also see that ~50% of reads are found within a cell. 
Cells that are captured have a high RNA content and large number of gene expressed (>4k).
The extra depth in replicates 2 and 3 provided roughly double the median number of genes per cell, even though the number of reads per cell increased modestly.
This would lead to an over all decrease in sparsity of the cell counts matrix.

#### V2 Force

Using the force option we can increase the number of cells per replicate to approach the theoretical 50% capture rate (**Table 1**).
However, the added cells have a lower UMI and gene content.
We still see an affect of increased depth with Replicates 2 and 3 having twice as many genes expressed per cell.
Again the number of reads per cell was not affected by sequencing depth.
Forcing also increased the fraction of reads found in a cell to ~60%.

#### V3 Defaults

The new version of cell ranger does call more cells than version 2 (defaults and force).
By adding so many low UMI cells, the replicate differences in gene content have all but disappeared.
While the fraction of reads per cell is still ~60%.
While this method is less *ad hoc* than V2 force, results are not appreciably better.


{{< tabs "summary stats" >}}

{{< tab "Cell Ranger (v2.1.1; defaults)" >}}

#### Testis Replicate 1
{{<figure src="cellranger2_testis1_summary.png" width="100%">}}

#### Testis Replicate 2
{{<figure src="cellranger2_testis2_summary.png" width="100%">}}

#### Testis Replicate 3
{{<figure src="cellranger2_testis3_summary.png" width="100%">}}

{{% button href="https://github.com/jfear/larval_gonad/blob/5f8f4569ea253ab96d497055e7ae6ebb4c82a744/scrnaseq-wf/Snakefile#L96-L121" %}}Code{{% /button %}}
{{< /tab >}}

<!-- ---------------------------------------------------- -->

{{< tab "Cell Ranger (v2.1.1; force)" >}}

#### Testis Replicate 1
{{<figure src="cellranger2_testis1_force_summary.png" width="100%">}}

#### Testis Replicate 2
{{<figure src="cellranger2_testis2_force_summary.png" width="100%">}}

#### Testis Replicate 3
{{<figure src="cellranger2_testis3_force_summary.png" width="100%">}}

{{% button href="https://github.com/jfear/larval_gonad/blob/5f8f4569ea253ab96d497055e7ae6ebb4c82a744/scrnaseq-wf/Snakefile#L123-L151" %}}Code{{% /button %}}
{{< /tab >}}

<!-- ---------------------------------------------------- -->

{{< tab "Cell Ranger (v3.0.1; defaults)" >}}

#### Testis Replicate 1
{{<figure src="cellranger3_testis1_summary.png" width="100%">}}

#### Testis Replicate 2
{{<figure src="cellranger3_testis2_summary.png" width="100%">}}

#### Testis Replicate 3
{{<figure src="cellranger3_testis3_summary.png" width="100%">}}

<!-- TODO: Change code link. -->
{{% button href="" %}}Code{{% /button %}}
{{< /tab >}}

<!-- ---------------------------------------------------- -->

{{< /tabs >}}

### Clustering

By looking at the cell ranger summary statistics we can begin to get a feel for the data.
However, it is hard to understand if the added cells are high quality and improve results. 
The next step in the analysis is clustering cells and identifying cell types. 
To get a better understanding of how cell selection affect clustering I ran one clustering method on each cell selection method.
For more details on clustering see [Cell Clustering]({{< relref path="../clustering" >}}).

{{< tabs "clustering" >}}

{{< tab "Cell Ranger (v2.1.1; defaults)" >}}
<!-- TODO: Update Section. -->


{{% button href="" %}}Code{{% /button %}}
{{< /tab >}}

<!-- ---------------------------------------------------- -->

{{< tab "Cell Ranger (v2.1.1; force)" >}}
<!-- TODO: Update Section. -->


{{% button href="" %}}Code{{% /button %}}
{{< /tab >}}

<!-- ---------------------------------------------------- -->

{{< tab "Cell Ranger (v3.0.1; defaults)" >}}
<!-- TODO: Update Section. -->


{{% button href="" %}}Code{{% /button %}}
{{< /tab >}}

<!-- ---------------------------------------------------- -->

{{< /tabs >}}


## Conclusion
