---
title: "10X Cell Selection"
weight: 1
# bookFlatSection: false
# bookShowToC: true
---

{{< figure src="https://cdn.technologynetworks.com/tn/images/thumbs/webp/640_360/10x-genomics-extends-their-application-portfolio-305346.webp?v=9720678" 
title="The 10X Library Preparation consists of mixing individual cells with individual gel beads.">}}

## Barcode Rank Plots

After sequencing, the first step in scRNA-Seq analysis is identifying cells. 
During library preparation, individual cells are mixed with individual beads. 
Each bead contains a unique barcode which is used for demultiplexing. 
The goal of cell selection is to find beads that had 1 and only 1 cell. 
Because cells are loaded in a double Poisson rate, the majority of beads will be empty. 
However, these empty cells may still have some signal due to RNA in the media. 
In rare cases a bead may be combined with 2 or more cells. 
This is particularly problematic when the 2+ cells are all from the same cell type.

10X Genomics provides their own software for demultiplexing and identifying cells (`Cell Ranger`).
This software does demultiplexes reads to the cell level.
Cells are called using total UMI count:

>Cell Ranger v3 ([website](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/algorithms/overview))

>1. Uses a cutoff based on total UMI counts of each barcode to identify cells. This step identifies the primary mode of high RNA content cells.
>2. Then the algorithm uses the RNA profile of each remaining barcode to determine if it is an “empty" or a cell containing partition. This second step captures low RNA content cells whose total UMI counts may be similar to empty GEMs.

>Cell Ranger v2 only uses step (1).

After cell selection `Cell Ranger` aligns reads using `STAR`.
It counts the number of reads per gene and provides summary statistics.

### Cell Ranger (v2.1.1) 
{{% button href="https://github.com/jfear/larval_gonad/blob/5f8f4569ea253ab96d497055e7ae6ebb4c82a744/scrnaseq-wf/Snakefile#L96-L121" %}}Code{{% /button %}}

10X Genomics suggests a capture rate of ~50% of cells loaded. We expect to capture at least 3,000 cells for each replicate. 
Using v2.1.1 of `Cell Ranger` with default settings we capture ~500 cells per replicate (below). 
The barcode rank plots indicate a long sloped decrease (left to right) with a knee appearing around 100 UMI.
We think `Cell Ranger` is under calling cells.
10X reported on their website that the v2 of their cell calling algorithm struggles when there is a high diversity of RNA content.
In our data we expect a high diversity of RNA content because we are looking at stem cells, germ cells, and somatic cells.
Therefore, we decided to try forcing the algorithm to call additional cells.

#### Testis Replicate 1
{{%expand "click to expand"%}}{{<figure src="cellranger2_testis1_summary.png">}}{{% /expand%}}

#### Testis Replicate 2
{{%expand "click to expand"%}}{{<figure src="cellranger2_testis2_summary.png">}}{{% /expand%}}

#### Testis Replicate 3
{{%expand "click to expand"%}}{{<figure src="cellranger2_testis3_summary.png">}}{{% /expand%}}

### Cell Ranger (v2.1.1; `--force-cells`)
{{% button href="https://github.com/jfear/larval_gonad/blob/5f8f4569ea253ab96d497055e7ae6ebb4c82a744/scrnaseq-wf/Snakefile#L123-L151" %}}Code{{% /button %}}

`Cell Ranger` provides a way to force a certain number of cells to be called.
We ran `Cell Ranger` using this option to force the calling of 3,000 cells for replicates 1 and 2 where we loaded ~6,000 cells. 
We loaded ~16,000 cells in replicate 3 so we force 8,000 cells to be called.
Again looking at the barcode rank plots we see that cells were called all the way to the middle of the linear portion of the distribution (~ ≥1,000 UMI).
The median total UMI per cell ranged from 1,091-5,429.
The median number of genes per cell ranged from 546-1,120. 
Replicates 2 and 3 had ~20,000,000 more reads than replicate 1. 

#### Testis Replicate 1
{{%expand "click to expand"%}}{{<figure src="cellranger2_testis1_force_summary.png">}}{{% /expand%}}

#### Testis Replicate 2
{{%expand "click to expand"%}}{{<figure src="cellranger2_testis2_force_summary.png">}}{{% /expand%}}

#### Testis Replicate 3
{{%expand "click to expand"%}}{{<figure src="cellranger2_testis3_force_summary.png">}}{{% /expand%}}

### Cell Ranger (v3.0.2)
<!-- TODO: Change code link. -->
{{% button href="https://github.com/jfear/larval_gonad/blob/5f8f4569ea253ab96d497055e7ae6ebb4c82a744/scrnaseq-wf/Snakefile#L123-L151" %}}Code{{% /button %}}

10X recently released v3 of their `Cell Ranger` software.
In this version they boast a better cell calling with low RNA content.  

<!-- TODO: add cellranger 3 figures. -->
#### Testis Replicate 1
{{%expand "click to expand"%}}{{<figure src="cellranger3_testis1_summary.png">}}{{% /expand%}}

#### Testis Replicate 2
{{%expand "click to expand"%}}{{<figure src="cellranger3_testis2_summary.png">}}{{% /expand%}}

#### Testis Replicate 3
{{%expand "click to expand"%}}{{<figure src="cellranger3_testis3_summary.png">}}{{% /expand%}}
