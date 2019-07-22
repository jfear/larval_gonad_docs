---
title: "Neo X Gene Movement"
weight: 3
# bookFlatSection: false
# bookShowToC: true
---

<span style="font-size: 3em; font-weight: 'strong'; color: #ff0000;">WARNING: Preliminary Analysis</span>

## Neo X Gene Movement

We are interested in the evolutionary impact of X inactivation.
There are several hypotheses about why the X would evolve MSCI during spermatogenesis.
Here we are focusing on the idea that if the X is really silenced during spermatogenesis, then genes important for male fertility should move off of the X.
During Drosophila genus evolution there was a fusion of Muller element A (X) with Muller element D (3L) in *D. pseudoobscura* (**Figure 1**).
Here we look at gene movement on *D. pseudoobscura* Muller element D.

{{<figure src="drosophila_synteny.png" width="100%"
caption="<b>Figure 1: Chromosome synteny across the Drosophila genus.</b> Image modified from http://flybase.org/maps/synteny.">}}

### Methods

#### Identification of orthologs

First, we looked at gene localization in *D. pseudoobscura*, *D. melanogaster*, *D. willistoni*, *D. mojavensis*, and *D. virilis*.
We downloaded updated gene annotations and ortholog tables from [GSE99574](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE99574).
We selected a set of 12,365 orthologous genes that are present in *D. melanogaster* and at least one of the other species.
In this annotation, genes from *D. willistoni*, *D. mojavensis*, and *D. virilis* are mapped to scaffolds instead of chromosome arm.
To map genes to Muller element we used a consensus approach.
For each scaffold, with > 20 genes, we lifted over the *D. melanogaster* Muller annotation using majority rule.
To validate our approach, we did this same process for *D. pseudoobscura* which has chromosomal annotation.
We achieved 100% agreement with *D. pseudoobscura* annotation.

#### Differential expression

### Results

**Table 1. Number of genes per Muller Element.**

|                         | Muller A | Muller D | Muller E |
|-------------------------|----------|----------|----------|
| <b>conserved</b>        | 1,079    | 1,235    | 1,735    |
| <b>recent_conserved</b> | 246      | 337      | 291      |
| <b>moved_on</b>         | 7        | 4        | 4        |
| <b>gene_death</b>       | 375      | 575      | 647      |
| <b>moved_off</b>        | 135      | 158      | 193      |
| <b>other</b>            | 284      | 386      | 468      |

{{< tabs "muller_arms" >}}
{{< tab "Muller A" >}}

{{<figure src="./2019-07-22_neox_analysis_boxplot_mullerA.svg" width="100%"
caption="<b>Figure 2. Gene movement on/off of Muller element A.</b> Selection Favor vs Selection Against: Mann-Whitney-U p-value: 1.32e-05">}}

**Table 2. Number of genes with differential expression on Muller A.**

| DEG   | conserved      | moved_on       | gene_death      | moved_off |
|-------|----------------|----------------|-----------------|-----------|
| NS    | 0<sup>\*</sup> | 1<sup>†</sup>  | 4               | 1         |
| cyte  | 1<sup>\*</sup> | 1              | 9<sup>†</sup>   | 1         |
| gonia | 62<sup>†</sup> | 0<sup>\*</sup> | 19<sup>\*</sup> | 4         |

<span style="font-size: .8em;">
    𝛘^2: 35.4513, p-value: 0.0000, df: 6,
    <sup>\*</sup>Significantly depleted (p-value ≤ 0.01),
    <sup>†</sup>Significantly enriched (p-value ≤ 0.01)
</span>

**Table 3. Number of genes with selection on Muller A.**

| DEG         | Selection Favor | Selection Against |
|-------------|-----------------|-------------------|
| Cyte Biased | 2               | 10                |
| Not Biased  | 63              | 28                |

* Fisher's exact test p-value: 0.0007

{{< /tab >}}
{{< tab "Muller D" >}}

{{<figure src="./2019-07-22_neox_analysis_boxplot_mullerD.svg" width="100%"
caption="<b>Figure 3. Gene movement on/off of Muller element D.</b> Selection Favor vs Selection Against: Mann-Whitney-U p-value: 4.78e-12">}}

**Table 4. Number of genes with differential expression on Muller D.**

| DEG   | measure  | conserved       | gene_death     | moved_off |
|-------|----------|-----------------|----------------|-----------|
| NS    | observed | 8               | 7              | 1         |
| cyte  | observed | 17<sup>\*</sup> | 27<sup>†</sup> | 7         |
| gonia | observed | 86<sup>†</sup>  | 7<sup>\*</sup> | 4         |

<span style="font-size: .8em;">
    𝛘^2: 50.6224, p-value: 0.0000, df: 4,
    <sup>\*</sup>Significantly depleted (p-value ≤ 0.01),
    <sup>†</sup>Significantly enriched (p-value ≤ 0.01)
</span>

**Table 5. Number of genes with selection on Muller D.**

| DEG         | Selection Favor | Selection Against |
|-------------|-----------------|-------------------|
| Cyte Biased | 17              | 34                |
| Not Biased  | 94              | 19                |

<span style="font-size: .8em;">
    Fisher's exact test p-value: 6.461e-10
    Mann-Whitney-U p-value: ≤ 0.001
</span>

{{< /tab >}}
{{< tab "Muller E" >}}

{{<figure src="./2019-07-22_neox_analysis_boxplot_mullerE.svg" width="100%"
caption="<b>Figure 4. Gene movement on/off of Muller element D.</b> Selection Favor vs Selection Against: Mann-Whitney-U p-value: 3.75e-17">}}

**Table 6. Number of genes with differential expression on Muller E.**

| DEG   | measure  | conserved       | moved_on | gene_death      | moved_off |
|-------|----------|-----------------|----------|-----------------|-----------|
| NS    | observed | 13              | 0        | 5               | 2         |
| cyte  | observed | 28<sup>\*</sup> | 1        | 28<sup>†</sup>  | 10        |
| gonia | observed | 117<sup>†</sup> | 0        | 12<sup>\*</sup> | 11        |

<span style="font-size: .8em;">
    𝛘^2: 41.5756, p-value: 0.0000, df: 6,
    <sup>\*</sup>Significantly depleted (p-value ≤ 0.01),
    <sup>†</sup>Significantly enriched (p-value ≤ 0.01)
</span>

**Table 7. Number of genes with selection on Muller E.**

| DEG         | Selection Favor | Selection Against |
|-------------|-----------------|-------------------|
| Cyte Biased | 29              | 38                |
| Not Biased  | 130             | 30                |

<span style="font-size: .8em;">
    Fisher's exact test p-value: 3.264e-08
</span>

{{< /tab >}}
{{< /tabs >}}