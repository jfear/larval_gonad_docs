---
title: "D. Pseudoobscura (SP vs Cytes)"
weight: 1
---

## *D. pseudoobscura* (SP vs Cytes)

I performed differential expression analysis by comparing cells in the SP cluster with cells in EPS, PS1, PS2, and PS3.
I then took the list of genes from this analysis (SP-biased, Cyte-biased, Not Significant) and filtered out the orthologs.
The majority of orthologs expressed in the germline were conserved with very few genes moving onto the respective Muller element (**Table 1**).

**Table 1. Number of genes per Muller Element.**

|                  |   muller_A |   muller_D |   muller_E |
|------------------|------------|------------|------------|
| conserved        |         76 |        145 |        212 |
| moved_on         |          8 |          1 |          4 |
| gene_death       |         31 |         22 |         25 |
| moved_off        |          5 |          8 |          7 |
| other            |         18 |         40 |         28 |

Using these conservation classes I wanted to see if there were differences in primary spermatocyte expression (EPS, PS1, PS2, PS3).
We can look for enrichment in two ways: (1) we can look at the percent of primary spermatocyte cells expressing genes in each class,
(2) we can use contingency tables to test to look for a relationship between expression bias and movement class.
If MSCI is driving gene movement, we expect that Muller A (X) and D (Neo-X) will have increased movement off for genes important for spermatogenesis.
As a control, Muller E (2) would not show selection.

### Primary Spermatocyte Expression

In **Figures 1-3** I look at the proportion of primary spermatocyte cells that expressed genes in the different movement classes.
The number of genes in the moved_on and moved_off classes are very low (< 10) which makes interpretation difficult.
I decided to group genes into "Selection Favor" and "Selection Against" for all statistical analyses.
Here I compare the distribution of the prorportion of cells using a Mann-Whitney U test.
**We see an significant increase in primary spermatocyte expression for genes with "Selection Against" for all three Muller elements.**


{{< tabs "cyte_dist" >}}
{{< tab "Muller A" >}}

#### Mann-Whitney U (Favor vs Against): 1.084447531668882e-09

{{<figure src="../neox_analysis_boxplot_gonia_vs_cytes_dpse_muller_A.svg" width="100%"
caption="<b>Figure 1. Gene movement on/off of Muller element A.</b> ">}}

{{< /tab >}}
{{< tab "Muller D" >}}

#### Mann-Whitney U (Favor vs Against): 5.792143929899031e-30

{{<figure src="../neox_analysis_boxplot_gonia_vs_cytes_dpse_muller_D.svg" width="100%"
caption="<b>Figure 2. Gene movement on/off of Muller element D.</b>">}}

{{< /tab >}}
{{< tab "Muller E" >}}

#### Mann-Whitney U (Favor vs Against): 4.6786534549032415e-51

{{<figure src="../neox_analysis_boxplot_gonia_vs_cytes_dpse_muller_E.svg" width="100%"
caption="<b>Figure 3. Gene movement on/off of Muller element E.</b>">}}

{{< /tab >}}
{{< /tabs >}}

### Enrichment of Primary Spermatocyte Biased Genes

Next, we looked at contingency tables to test for a relationship between primary spermatocyte biased expression and selection against.
Again, cell counts are very low for some combinations (**Table 2,4,6**) so I combined counts to make statistical analysis possible (**Table 3,5,7**).
Again there is evidence of a enrichment of genes that underwent "Selection Against" for all tested Muller elements.
While all Muller elements were significant, Muller D shows a more extreme pattern.

{{< tabs "crosstabs" >}}
{{< tab "Muller A" >}}

**Table 2. Number of genes with differential expression on Muller A.**

| bias   |   conserved |   moved_on |   gene_death |   moved_off |
|--------|-------------|------------|--------------|-------------|
| NS     |           0 |          2 |            6 |           1 |
| cyte   |           1 |          4 |           10 |           1 |
| gonia  |          75 |          2 |           15 |           3 |

**Table 3. Number of genes with selection on Muller A.**

| bias        |   Selection Favor |   Selection Against |
|-------------|-------------------|---------------------|
| Cyte Biased |                 5 |                  11 |
| Not Biased  |                79 |                  25 |

Fisher's Exact Test: 0.000682806162703328

{{< /tab >}}
{{< tab "Muller D" >}}

**Table 4. Number of genes with differential expression on Muller D.**

| bias   |   conserved |   moved_on |   gene_death |   moved_off |
|--------|-------------|------------|--------------|-------------|
| NS     |          12 |          0 |            4 |           1 |
| cyte   |          31 |          1 |           16 |           7 |
| gonia  |         102 |          0 |            2 |           0 |

**Table 5. Number of genes with selection on Muller D.**

| bias        |   Selection Favor |   Selection Against |
|-------------|-------------------|---------------------|
| Cyte Biased |                32 |                  23 |
| Not Biased  |               114 |                   7 |

Fisher's Exact Test: 2.016729661586391e-08

{{< /tab >}}
{{< tab "Muller E" >}}

**Table 6. Number of genes with differential expression on Muller E.**

| bias   |   conserved |   moved_on |   gene_death |   moved_off |
|--------|-------------|------------|--------------|-------------|
| NS     |          18 |          0 |            2 |           2 |
| cyte   |          49 |          3 |           17 |           5 |
| gonia  |         145 |          1 |            6 |           0 |

**Table 7. Number of genes with selection on Muller E.**

| bias        |   Selection Favor |   Selection Against |
|-------------|-------------------|---------------------|
| Cyte Biased |                52 |                  22 |
| Not Biased  |               164 |                  10 |

Fisher's Exact Test: 1.1519439830963736e-06

{{< /tab >}}
{{< /tabs >}}
