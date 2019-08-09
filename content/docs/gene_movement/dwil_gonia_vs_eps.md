---
title: "D. willistoni (Gonia vs EPS)"
weight: 6
---

#### D. williostoni (Gonia vs EPS)

I performed differential expression analysis by comparing cells in the SP cluster with cells in EPS.
I then took the list of genes from this analysis (SP-biased, EPS-biased, Not Significant) and filtered out the orthologs.
The majority of orthologs expressed in the germline were conserved with very few genes moving onto the respective Muller element (**Table 1**).

**Table 1. Number of genes per Muller Element.**

|                  |   muller_A |   muller_D |   muller_E |
|------------------|------------|------------|------------|
| conserved        |         46 |        100 |        141 |
| recent_conserved |          0 |          0 |          0 |
| moved_on         |          0 |          0 |          0 |
| gene_death       |         32 |         20 |         27 |
| moved_off        |          2 |         14 |          5 |
| other            |         27 |         32 |         26 |

Using these conservation classes I wanted to see if there were differences in EPS expression.
We can look for enrichment in two ways: (1) we can look at the percent of EPS cells expressing genes in each class,
(2) we can use contingency tables to test to look for a relationship between expression bias and movement class.
If MSCI is driving gene movement, we expect that Muller A (X) and D (Neo-X) will have increased movement off for genes important for spermatogenesis.
As a control, Muller E (2) would not show selection.

### EPS Expression

In **Figures 1-3** I look at the proportion of EPS cells that expressed genes in the different movement classes.
The number of genes in the moved_on and moved_off classes are very low (< 10) which makes interpretation difficult.
I decided to group genes into "Selection Favor" and "Selection Against" for all statistical analyses.
Here I compare the distribution of the proportion of cells using a Mann-Whitney U test.
**We see an significant increase in EPS expression for genes with "Selection Against" for all three Muller elements.**

{{< tabs "cyte_dist" >}}
{{< tab "Muller A" >}}

#### Mann-Whitney U (Favor vs Against): 0.0928171971739644

{{<figure src="../neox_analysis_boxplot_gonia_vs_eps_dwil_muller_A.svg" width="100%"
caption="<b>Figure 1. Gene movement on/off of Muller element A.</b> ">}}

{{< /tab >}}
{{< tab "Muller D" >}}

#### Mann-Whitney U (Favor vs Against): 3.567965845326442e-13

{{<figure src="../neox_analysis_boxplot_gonia_vs_eps_dwil_muller_D.svg" width="100%"
caption="<b>Figure 2. Gene movement on/off of Muller element D.</b>">}}

{{< /tab >}}
{{< tab "Muller E" >}}

#### Mann-Whitney U (Favor vs Against): 2.203276012631366e-25

{{<figure src="../neox_analysis_boxplot_gonia_vs_eps_dwil_muller_E.svg" width="100%"
caption="<b>Figure 3. Gene movement on/off of Muller element E.</b>">}}

{{< /tab >}}
{{< /tabs >}}

#### Enrichment of EPS biased genes

Next, we looked at contingency tables to test for a relationship between EPS biased expression and selection against.
Again, cell counts are very low for some combinations (**Table 2,4,6**) so I combined counts to make statistical analysis possible (**Table 3,5,7**).
Again there is evidence of a enrichment of genes that underwent "Selection Against" for all tested Muller elements.

{{< tabs "crosstabs" >}}
{{< tab "Muller A" >}}

**Table 2. Number of genes with differential expression on Muller A.**

| bias | conserved | moved_on | gene_death | moved_off |
|------|-----------|----------|------------|-----------|
| NS   | 5         | 0        | 7          | 0         |
| EPS  | 1         | 0        | 15         | 1         |
| SP   | 40        | 0        | 10         | 1         |

**Table 3. Number of genes with selection on Muller A.**

| bias       | Selection Favor | Selection Against |
|------------|-----------------|-------------------|
| EPS Biased | 1               | 16                |
| Not Biased | 45              | 18                |

Fisher's Exact Test: 1.0219338190613155e-06

{{< /tab >}}
{{< tab "Muller D" >}}

**Table 4. Number of genes with differential expression on Muller D.**

| bias | conserved | moved_on | gene_death | moved_off |
|------|-----------|----------|------------|-----------|
| NS   | 17        | 0        | 6          | 7         |
| EPS  | 27        | 0        | 13         | 6         |
| SP   | 56        | 0        | 1          | 1         |

**Table 5. Number of genes with selection on Muller D.**

| bias       | Selection Favor | Selection Against |
|------------|-----------------|-------------------|
| EPS Biased | 27              | 19                |
| Not Biased | 73              | 15                |

Fisher's Exact Test: 0.0032662535424320812

{{< /tab >}}
{{< tab "Muller E" >}}

**Table 6. Number of genes with differential expression on Muller E.**

| bias | conserved | moved_on | gene_death | moved_off |
|------|-----------|----------|------------|-----------|
| NS   | 19        | 0        | 6          | 1         |
| EPS  | 44        | 0        | 17         | 3         |
| SP   | 78        | 0        | 4          | 1         |

**Table 7. Number of genes with selection on Muller E.**

| bias       | Selection Favor | Selection Against |
|------------|-----------------|-------------------|
| EPS Biased | 44              | 20                |
| Not Biased | 97              | 12                |

Fisher's Exact Test: 0.0019497319372953786

{{< /tab >}}
{{< /tabs >}}
