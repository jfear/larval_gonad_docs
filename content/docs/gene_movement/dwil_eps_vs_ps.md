---
title: "D. willistoni (EPS vs PS)"
weight: 7
---

#### D. williostoni (EPS vs PS)

I performed differential expression analysis by comparing cells in the EPS cluster with cells in PS1, PS2, and PS3.
I then took the list of genes from this analysis (EPS-biased, PS-biased, Not Significant) and filtered out the orthologs.
The majority of orthologs expressed in the germline were conserved with very few genes moving onto the respective Muller element (**Table 1**).

**Table 1. Number of genes per Muller Element.**

| index            | muller_A | muller_D | muller_E |
|------------------|----------|----------|----------|
| conserved        | 54       | 114      | 177      |
| recent_conserved | 0        | 0        | 0        |
| moved_on         | 0        | 0        | 0        |
| gene_death       | 32       | 19       | 30       |
| moved_off        | 2        | 14       | 6        |
| other            | 24       | 30       | 26       |

Using these conservation classes I wanted to see if there were differences in PS expression.
We can look for enrichment in two ways: (1) we can look at the percent of PS cells expressing genes in each class,
(2) we can use contingency tables to test to look for a relationship between expression bias and movement class.
If MSCI is driving gene movement, we expect that Muller A (X) and D (Neo-X) will have increased movement off for genes important for spermatogenesis.
As a control, Muller E (2) would not show selection.

### PS Expression

In **Figures 1-3** I look at the proportion of PS cells that expressed genes in the different movement classes.
The number of genes in the moved_on and moved_off classes are very low (< 10) which makes interpretation difficult.
I decided to group genes into "Selection Favor" and "Selection Against" for all statistical analyses.
Here I compare the distribution of the proportion of cells using a Mann-Whitney U test.
**We see an significant increase in PS expression for genes with "Selection Against" for all three Muller elements.**

{{< tabs "cyte_dist" >}}
{{< tab "Muller A" >}}

#### Mann-Whitney U (Favor vs Against): 0.005195675758492797

{{<figure src="../neox_analysis_boxplot_eps_vs_ps_dwil_muller_A.svg" width="100%"
caption="<b>Figure 1. Gene movement on/off of Muller element A.</b> ">}}

{{< /tab >}}
{{< tab "Muller D" >}}

#### Mann-Whitney U (Favor vs Against): 1.1210518575758348e-17

{{<figure src="../neox_analysis_boxplot_eps_vs_ps_dwil_muller_D.svg" width="100%"
caption="<b>Figure 2. Gene movement on/off of Muller element D.</b>">}}

{{< /tab >}}
{{< tab "Muller E" >}}

#### Mann-Whitney U (Favor vs Against): 1.3127729254231856e-34

{{<figure src="../neox_analysis_boxplot_eps_vs_ps_dwil_muller_E.svg" width="100%"
caption="<b>Figure 3. Gene movement on/off of Muller element E.</b>">}}

{{< /tab >}}
{{< /tabs >}}

#### Enrichment of PS biased genes

Next, we looked at contingency tables to test for a relationship between PS biased expression and selection against.
Again, cell counts are very low for some combinations (**Table 2,4,6**) so I combined counts to make statistical analysis possible (**Table 3,5,7**).
Again there is evidence of a enrichment of genes that underwent "Selection Against" for Muller elements D and E.

{{< tabs "crosstabs" >}}
{{< tab "Muller A" >}}

**Table 2. Number of genes with differential expression on Muller A.**

| bias | conserved | moved_on | gene_death | moved_off |
|------|-----------|----------|------------|-----------|
| NS   | 4         | 0        | 3          | 1         |
| PS   | 0         | 0        | 3          | 0         |
| EPS  | 50        | 0        | 26         | 1         |

**Table 3. Number of genes with selection on Muller A.**

| bias       | Selection Favor | Selection Against |
|------------|-----------------|-------------------|
| PS Biased  | 0               | 3                 |
| Not Biased | 54              | 31                |

Fisher's Exact Test: 0.05453087409783467

{{< /tab >}}
{{< tab "Muller D" >}}

**Table 4. Number of genes with differential expression on Muller D.**

| bias | conserved | moved_on | gene_death | moved_off |
|------|-----------|----------|------------|-----------|
| NS   | 9         | 0        | 3          | 4         |
| PS   | 23        | 0        | 8          | 7         |
| EPS  | 82        | 0        | 8          | 3         |

**Table 5. Number of genes with selection on Muller D.**

| bias       | Selection Favor | Selection Against |
|------------|-----------------|-------------------|
| PS Biased  | 23              | 15                |
| Not Biased | 91              | 18                |

Fisher's Exact Test: 0.006088378575605279

{{< /tab >}}
{{< tab "Muller E" >}}

**Table 6. Number of genes with differential expression on Muller E.**

| bias | conserved | moved_on | gene_death | moved_off |
|------|-----------|----------|------------|-----------|
| NS   | 19        | 0        | 4          | 0         |
| PS   | 42        | 0        | 16         | 4         |
| EPS  | 116       | 0        | 10         | 2         |

**Table 6. Number of genes with differential expression on Muller E.**

| bias       | Selection Favor | Selection Against |
|------------|-----------------|-------------------|
| PS Biased  | 42              | 20                |
| Not Biased | 135             | 16                |

Fisher's Exact Test: 0.00043644429109212303

{{< /tab >}}
{{< /tabs >}}
