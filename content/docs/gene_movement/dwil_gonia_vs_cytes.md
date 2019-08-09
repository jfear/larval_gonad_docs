---
title: "D. willistoni (Gonia vs Cytes)"
weight: 5
---

#### D. williostoni (Gonia vs Cytes)

I performed differential expression analysis by comparing cells in the SP cluster with cells in EPS, PS1, PS2, and PS3.
I then took the list of genes from this analysis (SP-biased, Cyte-biased, Not Significant) and filtered out the orthologs.
The majority of orthologs expressed in the germline were conserved with very few genes moving onto the respective Muller element (**Table 1**).

**Table 1. Number of genes per Muller Element.**

| index            |   muller_A |   muller_D |   muller_E |
|------------------|------------|------------|------------|
| conserved        |         76 |        145 |        212 |
| moved_on         |          0 |          0 |          0 |
| gene_death       |         30 |         22 |         32 |
| moved_off        |          2 |         16 |          6 |
| other            |         30 |         33 |         26 |

Using these conservation classes I wanted to see if there were differences in primary spermatocyte expression (EPS, PS1, PS2, PS3).
We can look for enrichment in two ways: (1) we can look at the percent of primary spermatocyte cells expressing genes in each class,
(2) we can use contingency tables to test to look for a relationship between expression bias and movement class.
If MSCI is driving gene movement, we expect that Muller A (X) and D (Neo-X) will have increased movement off for genes important for spermatogenesis.
As a control, Muller E (2) would not show selection.

### Primary Spermatocyte Expression

In **Figures 1-3** I look at the proportion of primary spermatocyte cells that expressed genes in the different movement classes.
The number of genes in the moved_on and moved_off classes are very low (< 10) which makes interpretation difficult.
I decided to group genes into "Selection Favor" and "Selection Against" for all statistical analyses.
Here I compare the distribution of the proportion of cells using a Mann-Whitney U test.
**We see an significant increase in primary spermatocyte expression for genes with "Selection Against" for all three Muller elements.**

{{< tabs "cyte_dist" >}}
{{< tab "Muller A" >}}

#### Mann-Whitney U (Favor vs Against): 6.128477178673135e-10

{{<figure src="../neox_analysis_boxplot_gonia_vs_cytes_dwil_muller_A.svg" width="100%"
caption="<b>Figure 1. Gene movement on/off of Muller element A.</b> ">}}

{{< /tab >}}
{{< tab "Muller D" >}}

#### Mann-Whitney U (Favor vs Against): 1.2297322858955786e-25

{{<figure src="../neox_analysis_boxplot_gonia_vs_cytes_dwil_muller_D.svg" width="100%"
caption="<b>Figure 2. Gene movement on/off of Muller element D.</b>">}}

{{< /tab >}}
{{< tab "Muller E" >}}

#### Mann-Whitney U (Favor vs Against): 8.313249967597653e-45

{{<figure src="../neox_analysis_boxplot_gonia_vs_cytes_dwil_muller_E.svg" width="100%"
caption="<b>Figure 3. Gene movement on/off of Muller element E.</b>">}}

{{< /tab >}}
{{< /tabs >}}

#### Enrichment of primary spermatocyte biased genes

Next, we looked at contingency tables to test for a relationship between primary spermatocyte biased expression and selection against.
Again, cell counts are very low for some combinations (**Table 2,4,6**) so I combined counts to make statistical analysis possible (**Table 3,5,7**).
Again there is evidence of a enrichment of genes that underwent "Selection Against" for all tested Muller elements.

{{< tabs "crosstabs" >}}
{{< tab "Muller A" >}}

**Table 2. Number of genes with differential expression on Muller A.**

| bias   |   conserved |   moved_on |   gene_death |   moved_off |
|--------|-------------|------------|--------------|-------------|
| NS     |           0 |          0 |            6 |           0 |
| cyte   |           1 |          0 |            9 |           1 |
| gonia  |          75 |          0 |           15 |           1 |

**Table 3. Number of genes with selection on Muller A.**

| bias        |   Selection Favor |   Selection Against |
|-------------|-------------------|---------------------|
| Cyte Biased |                 1 |                  10 |
| Not Biased  |                75 |                  22 |

Fisher's Exact Test: 1.4586005236732388e-05

{{< /tab >}}
{{< tab "Muller D" >}}

**Table 4. Number of genes with differential expression on Muller D.**

| bias   |   conserved |   moved_on |   gene_death |   moved_off |
|--------|-------------|------------|--------------|-------------|
| NS     |          12 |          0 |            5 |           4 |
| cyte   |          31 |          0 |           13 |           9 |
| gonia  |         102 |          0 |            4 |           3 |

**Table 5. Number of genes with selection on Muller D.**

| bias        |   Selection Favor |   Selection Against |
|-------------|-------------------|---------------------|
| Cyte Biased |                31 |                  22 |
| Not Biased  |               114 |                  16 |

Fisher's Exact Test: 3.1710112640960124e-05

{{< /tab >}}
{{< tab "Muller E" >}}

**Table 6. Number of genes with differential expression on Muller E.**

| bias   |   conserved |   moved_on |   gene_death |   moved_off |
|--------|-------------|------------|--------------|-------------|
| NS     |          18 |          0 |            5 |           2 |
| cyte   |          49 |          0 |           21 |           3 |
| gonia  |         145 |          0 |            6 |           1 |

**Table 7. Number of genes with selection on Muller E.**

| bias        |   Selection Favor |   Selection Against |
|-------------|-------------------|---------------------|
| Cyte Biased |                49 |                  24 |
| Not Biased  |               163 |                  14 |

Fisher's Exact Test: 2.706475148827376e-06

{{< /tab >}}
{{< /tabs >}}
