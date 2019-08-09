---
title: "D. Pseudoobscura (EPS vs PS)"
weight: 4
---

## *D. pseudoobscura* (EPS vs PS)

I performed differential expression analysis by comparing cells in the EPS cluster with cells in PS1, PS2, and PS3.
I then took the list of genes from this analysis (EPS-biased, PS-biased, Not Significant) and filtered out the orthologs.
The number of genes analyzed per Muller element and class are in **Table 1**.

**Table 1. Number of genes per Muller Element.**

|                  |   muller_A |   muller_D |   muller_E |
|------------------|------------|------------|------------|
| conserved        |         54 |        114 |        177 |
| recent_conserved |          0 |          0 |          0 |
| moved_on         |          7 |          1 |          3 |
| gene_death       |         34 |         20 |         24 |
| moved_off        |          3 |          8 |          6 |
| other            |         14 |         34 |         29 |

### PS Expression

In **Figures 1-3** I look at the proportion of PS1, PS2, and P3 cells that expressed genes in the different movement classes.
The number of genes in the moved_on and moved_off classes are very low (< 10) which makes interpretation difficult.
I group genes into "Selection Favor" and "Selection Against" for all statistical analyses.
Here I compare the distribution of the proportion of cells using a Mann-Whitney U test.
**We see an significant increase in PS expression for genes with "Selection Against" on all Muller elements.**
However, the number of genes in the two classes is very different so I am worried about biases.

{{< tabs "cyte_dist" >}}
{{< tab "Muller A" >}}

#### Mann-Whitney U (Favor vs Against): 0.00014820920368461394

{{<figure src="../neox_analysis_boxplot_eps_vs_ps_dpse_muller_A.svg" width="100%"
caption="<b>Figure 1. Gene movement on/off of Muller element A.</b> ">}}

{{< /tab >}}
{{< tab "Muller D" >}}

#### Mann-Whitney U (Favor vs Against): 4.247933807388154e-20

{{<figure src="../neox_analysis_boxplot_eps_vs_ps_dpse_muller_D.svg" width="100%"
caption="<b>Figure 2. Gene movement on/off of Muller element D.</b>">}}

{{< /tab >}}
{{< tab "Muller E" >}}

#### Mann-Whitney U (Favor vs Against): 4.278607873507591e-40

{{<figure src="../neox_analysis_boxplot_eps_vs_ps_dpse_muller_E.svg" width="100%"
caption="<b>Figure 3. Gene movement on/off of Muller element E.</b>">}}

{{< /tab >}}
{{< /tabs >}}

### Enrichment of PS Biased Genes

Next, we looked at contingency tables to test for a relationship between PS1, PS2, and PS3 biased expression and selection against.
Again, cell counts are very low for some combinations (**Table 2,4,6**) so I combined counts to make statistical analysis possible (**Table 3,5,7**).
There is evidence of a enrichment of genes that underwent "Selection Against" for Muller elements D and E.

{{< tabs "crosstabs" >}}
{{< tab "Muller A" >}}

**Table 2. Number of genes with differential expression on Muller A.**

| bias | conserved | moved_on | gene_death | moved_off |
|------|-----------|----------|------------|-----------|
| NS   | 4         | 0        | 3          | 1         |
| PS   | 0         | 4        | 4          | 0         |
| EPS  | 50        | 3        | 27         | 2         |

**Table 3. Number of genes with selection on Muller A.**

| bias       | Selection Favor | Selection Against |
|------------|-----------------|-------------------|
| PS Biased  | 4               | 4                 |
| Not Biased | 57              | 33                |

Fisher's Exact Test: 0.47126741340750544

{{< /tab >}}
{{< tab "Muller D" >}}

**Table 4. Number of genes with differential expression on Muller D.**

| bias | conserved | moved_on | gene_death | moved_off |
|------|-----------|----------|------------|-----------|
| NS   | 9         | 0        | 5          | 0         |
| PS   | 23        | 1        | 8          | 6         |
| EPS  | 82        | 0        | 7          | 2         |

**Table 5. Number of genes with selection on Muller D.**

| bias       | Selection Favor | Selection Against |
|------------|-----------------|-------------------|
| PS Biased  | 24              | 14                |
| Not Biased | 91              | 14                |

Fisher's Exact Test: 0.0034996761816397386

{{< /tab >}}
{{< tab "Muller E" >}}

**Table 6. Number of genes with differential expression on Muller E.**

| bias | conserved | moved_on | gene_death | moved_off |
|------|-----------|----------|------------|-----------|
| NS   | 19        | 0        | 2          | 3         |
| PS   | 42        | 2        | 15         | 1         |
| EPS  | 116       | 1        | 7          | 2         |

**Table 7. Number of genes with selection on Muller E.**

| bias       | Selection Favor | Selection Against |
|------------|-----------------|-------------------|
| PS Biased  | 44              | 16                |
| Not Biased | 136             | 14                |

Fisher's Exact Test: 0.0020621301227712065

{{< /tab >}}
{{< /tabs >}}
