---
title: "D. willistoni (SP vs PS)"
weight: 6
---

#### D. williostoni (SP vs PS)

I performed differential expression analysis by comparing cells in the SP cluster with cells in PS1, PS2, PS3.
I then took the list of genes from this analysis (SP-biased, PS-biased, Not Significant) and filtered out the orthologs.
The majority of orthologs expressed in the germline were conserved with very few genes moving onto the respective Muller element (**Table 1**).

**Table 1. Number of genes per Muller Element.**

| index            | muller_A | muller_D | muller_E |
|------------------|----------|----------|----------|
| conserved        | 80       | 151      | 218      |
| moved_on         | 0        | 0        | 0        |
| gene_death       | 30       | 22       | 34       |
| moved_off        | 2        | 18       | 6        |
| other            | 28       | 34       | 26       |

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

#### Mann-Whitney U (Favor vs Against): 1.247889494162566e-10

{{<figure src="../neox_analysis_boxplot_gonia_vs_ps_dwil_muller_A.svg" width="100%"
caption="<b>Figure 1. Gene movement on/off of Muller element A.</b> ">}}

{{< /tab >}}
{{< tab "Muller D" >}}

#### Mann-Whitney U (Favor vs Against): 4.356009043322584e-24

{{<figure src="../neox_analysis_boxplot_gonia_vs_ps_dwil_muller_D.svg" width="100%"
caption="<b>Figure 2. Gene movement on/off of Muller element D.</b>">}}

{{< /tab >}}
{{< tab "Muller E" >}}

#### Mann-Whitney U (Favor vs Against): 3.009253790452862e-45

{{<figure src="../neox_analysis_boxplot_gonia_vs_ps_dwil_muller_E.svg" width="100%"
caption="<b>Figure 3. Gene movement on/off of Muller element E.</b>">}}

{{< /tab >}}
{{< /tabs >}}

#### Enrichment of PS biased genes

Next, we looked at contingency tables to test for a relationship between PS biased expression and selection against.
Again, cell counts are very low for some combinations (**Table 2,4,6**) so I combined counts to make statistical analysis possible (**Table 3,5,7**).
Again there is evidence of a enrichment of genes that underwent "Selection Against" for all tested Muller elements.

{{< tabs "crosstabs" >}}
{{< tab "Muller A" >}}

**Table 2. Number of genes with differential expression on Muller A.**

| bias | conserved | moved_on | gene_death | moved_off |
|------|-----------|----------|------------|-----------|
| NS   | 0         | 0        | 7          | 0         |
| PS   | 1         | 0        | 5          | 1         |
| SP   | 79        | 0        | 18         | 1         |

**Table 3. Number of genes with selection on Muller A.**

| bias       | Selection Favor | Selection Against |
|------------|-----------------|-------------------|
| PS Biased  | 1               | 6                 |
| Not Biased | 79              | 26                |

Fisher's Exact Test: 0.002094000366233636

{{< /tab >}}
{{< tab "Muller D" >}}

**Table 4. Number of genes with differential expression on Muller D.**

| bias | conserved | moved_on | gene_death | moved_off |
|------|-----------|----------|------------|-----------|
| NS   | 12        | 0        | 3          | 3         |
| PS   | 31        | 0        | 13         | 10        |
| SP   | 108       | 0        | 6          | 5         |

**Table 5. Number of genes with selection on Muller D.**

| bias       | Selection Favor | Selection Against |
|------------|-----------------|-------------------|
| PS Biased  | 31              | 23                |
| Not Biased | 120             | 17                |

Fisher's Exact Test: 1.6180277946244638e-05

{{< /tab >}}
{{< tab "Muller E" >}}

**Table 6. Number of genes with differential expression on Muller E.**

| bias | conserved | moved_on | gene_death | moved_off |
|------|-----------|----------|------------|-----------|
| NS   | 16        | 0        | 5          | 2         |
| PS   | 50        | 0        | 21         | 3         |
| SP   | 152       | 0        | 8          | 1         |

**Table 7. Number of genes with selection on Muller E.**

| bias       | Selection Favor | Selection Against |
|------------|-----------------|-------------------|
| PS Biased  | 50              | 24                |
| Not Biased | 168             | 16                |

Fisher's Exact Test: 6.496432841647948e-06

{{< /tab >}}
{{< /tabs >}}
