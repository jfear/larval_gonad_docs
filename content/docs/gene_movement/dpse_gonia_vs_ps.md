---
title: "D. Pseudoobscura (SP vs PS)"
weight: 3
---

## *D. pseudoobscura* (SP vs PS)

I performed differential expression analysis by comparing cells in the SP cluster with cells in PS1, PS2, and PS3.
I then took the list of genes from this analysis (SP-biased, PS-biased, Not Significant) and filtered out the orthologs.
The number of genes analyzed per Muller element and class are in **Table 1**.

**Table 1. Number of genes per Muller Element.**

| index            |   muller_A |   muller_D |   muller_E |
|------------------|------------|------------|------------|
| conserved        |         80 |        151 |        218 |
| moved_on         |          7 |          1 |          4 |
| gene_death       |         31 |         21 |         27 |
| moved_off        |          4 |          9 |          6 |
| other            |         18 |         43 |         29 |

### PS Expression

In **Figures 1-3** I look at the proportion of PS1, PS2, and P3 cells that expressed genes in the different movement classes.
The number of genes in the moved_on and moved_off classes are very low (< 10) which makes interpretation difficult.
I group genes into "Selection Favor" and "Selection Against" for all statistical analyses.
Here I compare the distribution of the proportion of cells using a Mann-Whitney U test.
**We see an significant increase in PS expression for genes with "Selection Against" on all Muller elements.**
However, the number of genes in the two classes is very different so I am worried about biases.

{{< tabs "cyte_dist" >}}
{{< tab "Muller A" >}}

#### Mann-Whitney U (Favor vs Against): 4.4927665471754445e-11

{{<figure src="../neox_analysis_boxplot_gonia_vs_ps_dpse_muller_A.svg" width="100%"
caption="<b>Figure 1. Gene movement on/off of Muller element A.</b> ">}}

{{< /tab >}}
{{< tab "Muller D" >}}

#### Mann-Whitney U (Favor vs Against): 6.832147521615045e-31

{{<figure src="../neox_analysis_boxplot_gonia_vs_ps_dpse_muller_D.svg" width="100%"
caption="<b>Figure 2. Gene movement on/off of Muller element D.</b>">}}

{{< /tab >}}
{{< tab "Muller E" >}}

#### Mann-Whitney U (Favor vs Against): 1.1474515219576066e-51

{{<figure src="../neox_analysis_boxplot_gonia_vs_ps_dpse_muller_E.svg" width="100%"
caption="<b>Figure 3. Gene movement on/off of Muller element E.</b>">}}

{{< /tab >}}
{{< /tabs >}}

### Enrichment of PS Biased Genes

Next, we looked at contingency tables to test for a relationship between PS1, PS2, and PS3 biased expression and selection against.
Again, cell counts are very low for some combinations (**Table 2,4,6**) so I combined counts to make statistical analysis possible (**Table 3,5,7**).
There is evidence of a enrichment of genes that underwent "Selection Against" for all tested Muller elements.

{{< tabs "crosstabs" >}}
{{< tab "Muller A" >}}

**Table 2. Number of genes with differential expression on Muller A.**

| bias | conserved | moved_on | gene_death | moved_off |
|------|-----------|----------|------------|-----------|
| NS   | 0         | 1        | 7          | 0         |
| PS   | 1         | 4        | 7          | 1         |
| SP   | 79        | 2        | 17         | 3         |

**Table 3. Number of genes with selection on Muller A.**

| bias       | Selection Favor | Selection Against |
|------------|-----------------|-------------------|
| PS Biased  | 5               | 8                 |
| Not Biased | 82              | 27                |

Fisher's Exact Test: 0.009540858896492865

{{< /tab >}}
{{< tab "Muller D" >}}

**Table 4. Number of genes with differential expression on Muller A.**

| bias | conserved | moved_on | gene_death | moved_off |
|------|-----------|----------|------------|-----------|
| NS   | 12        | 0        | 2          | 1         |
| PS   | 31        | 1        | 16         | 7         |
| SP   | 108       | 0        | 3          | 1         |

**Table 5. Number of genes with selection on Muller A.**

| bias       | Selection Favor | Selection Against |
|------------|-----------------|-------------------|
| PS Biased  | 32              | 23                |
| Not Biased | 120             | 7                 |

Fisher's Exact Test: 9.485216138214068e-09

{{< /tab >}}
{{< tab "Muller E" >}}

**Table 6. Number of genes with differential expression on Muller A.**

| bias | conserved | moved_on | gene_death | moved_off |
|------|-----------|----------|------------|-----------|
| NS   | 16        | 0        | 3          | 1         |
| PS   | 50        | 3        | 17         | 5         |
| SP   | 152       | 1        | 7          | 0         |

**Table 7. Number of genes with selection on Muller A.**

| bias       | Selection Favor | Selection Against |
|------------|-----------------|-------------------|
| PS Biased  | 53              | 22                |
| Not Biased | 169             | 11                |

Fisher's Exact Test: 2.068291103314802e-06

{{< /tab >}}
{{< /tabs >}}
