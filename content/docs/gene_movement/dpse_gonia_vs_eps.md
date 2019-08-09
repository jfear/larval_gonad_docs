---
title: "D. Pseudoobscura (SP vs EPS)"
weight: 2
---

## *D. pseudoobscura* (SP vs EPS)

I performed differential expression analysis by comparing cells in the SP cluster with cells in EPS.
I then took the list of genes from this analysis (SP-biased, EPS-biased, Not Significant) and filtered out the orthologs.
The number of genes analyzed per Muller element and class are in **Table 1**.

**Table 1. Number of genes per Muller Element.**

|                  |   muller_A |   muller_D |   muller_E |
|------------------|------------|------------|------------|
| conserved        |         46 |        100 |        141 |
| moved_on         |          7 |          0 |          4 |
| gene_death       |         34 |         23 |         20 |
| moved_off        |          4 |          9 |          7 |
| other            |         16 |         34 |         27 |


### Primary Spermatocyte Expression

In **Figures 1-3** I look at the proportion of EPS cells that expressed genes in the different movement classes.
The number of genes in the moved_on and moved_off classes are very low (< 10) which makes interpretation difficult.
I group genes into "Selection Favor" and "Selection Against" for all statistical analyses.
Here I compare the distribution of the proportion of cells using a Mann-Whitney U test.
**We see an significant increase in EPS expression for genes with "Selection Against" on Muller elements D and E.**
However, the number of genes in the two classes is very different so I am worried about biases.

{{< tabs "cyte_dist" >}}
{{< tab "Muller A" >}}

#### Mann-Whitney U (Favor vs Against): 0.06408074884649775

{{<figure src="../neox_analysis_boxplot_gonia_vs_eps_dpse_muller_A.svg" width="100%"
caption="<b>Figure 1. Gene movement on/off of Muller element A.</b> ">}}

{{< /tab >}}
{{< tab "Muller D" >}}

#### Mann-Whitney U (Favor vs Against): 5.421007642766501e-16

{{<figure src="../neox_analysis_boxplot_gonia_vs_eps_dpse_muller_D.svg" width="100%"
caption="<b>Figure 2. Gene movement on/off of Muller element D.</b>">}}

{{< /tab >}}
{{< tab "Muller E" >}}

#### Mann-Whitney U (Favor vs Against): 4.400211255192757e-30

{{<figure src="../neox_analysis_boxplot_gonia_vs_eps_dpse_muller_E.svg" width="100%"
caption="<b>Figure 3. Gene movement on/off of Muller element E.</b>">}}

{{< /tab >}}
{{< /tabs >}}

### Enrichment of EPS Biased Genes

Next, we looked at contingency tables to test for a relationship between EPS biased expression and selection against.
Again, cell counts are very low for some combinations (**Table 2,4,6**) so I combined counts to make statistical analysis possible (**Table 3,5,7**).
Again there is evidence of a enrichment of genes that underwent "Selection Against" for all tested Muller elements.

{{< tabs "crosstabs" >}}
{{< tab "Muller A" >}}

**Table 2. Number of genes with differential expression on Muller A.**

| bias   |   conserved |   moved_on |   gene_death |   moved_off |
|--------|-------------|------------|--------------|-------------|
| NS     |           5 |          1 |            8 |           0 |
| EPS    |           1 |          5 |           16 |           2 |
| SP     |          40 |          1 |           10 |           2 |

**Table 3. Number of genes with selection on Muller A.**

| bias       | Selection Favor | Selection Against |
|------------|-----------------|-------------------|
| EPS Biased | 6               | 18                |
| Not Biased | 47              | 20                |

Fisher's Exact Test: 0.0002062765044224908

{{< /tab >}}
{{< tab "Muller D" >}}

**Table 4. Number of genes with differential expression on Muller D.**

| bias | conserved | moved_on | gene_death | moved_off |
|------|-----------|----------|------------|-----------|
| NS   | 17        | 0        | 7          | 3         |
| EPS  | 27        | 0        | 14         | 6         |
| SP   | 56        | 0        | 2          | 0         |

**Table 5. Number of genes with selection on Muller D.**

| bias       | Selection Favor | Selection Against |
|------------|-----------------|-------------------|
| EPS Biased | 27              | 20                |
| Not Biased | 73              | 12                |

Fisher's Exact Test: 0.0005337227613258306

{{< /tab >}}
{{< tab "Muller E" >}}

**Table 6. Number of genes with differential expression on Muller E.**

| bias | conserved | moved_on | gene_death | moved_off |
|------|-----------|----------|------------|-----------|
| NS   | 19        | 2        | 3          | 1         |
| EPS  | 44        | 1        | 14         | 6         |
| SP   | 78        | 1        | 3          | 0         |

**Table 7. Number of genes with selection on Muller E.**

| bias       | Selection Favor | Selection Against |
|------------|-----------------|-------------------|
| EPS Biased | 45              | 20                |
| Not Biased | 100             | 7                 |

Fisher's Exact Test: 5.007160270119653e-05

{{< /tab >}}
{{< /tabs >}}
