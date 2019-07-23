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
Here we focus on the idea that genes important for spermatogenesis will move off of the X to avoid silencing.
To investigate this question, we look at gene movement off of the Neo X chromosome in *D. pseudoobscura*.
In *D. pseudoobscura* Muller element A (X) fused with Muller element D (3L) (**Figure 1**).
We expect that genes important for spermatogenesis, on Muller element D, will move to another chromosome or be lost.

{{<figure src="drosophila_synteny.png" width="100%"
caption="<b>Figure 1: Chromosome synteny across the Drosophila genus.</b> Image modified from http://flybase.org/maps/synteny.">}}

First, we need to characterize gene movement in the Drosophila genus.
Then we can look for enrichment in gene movement off of Muller D in genes up regulated in primary spermatocytes.
To characterize gene movement we look at gene localization in a subset of the Drosophila genus.
Here we focus on 5 members of the genus: *D. pseudoobscura*, *D. melanogaster*, *D. willistoni*, *D. mojavensis*, and *D. virilis*.
We downloaded updated gene annotations and ortholog mappings from [GSE99574](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE99574).
We selected a set of 12,365 orthologous genes present in *D. melanogaster* and at least one of the other species.
Using these gene locations we can determine how the gene is behaving in *D. pseudoobscura*.

### Methods

#### Identification of orthologs

The quality of assemblies and annotations are highly variable across the Drosophila genus.
We found that genes from *D. willistoni*, *D. mojavensis*, and *D. virilis* were mapped to scaffolds instead of chromosome arm.
We used annotations from *D. melanogaster* to assign Muller element to each of these scaffolds using a consensus approach.
For each scaffold, with > 20 genes, we looked where their orthologs were located in *D. melanogaster*.
We then assigned the Muller element using majority rule.
To validate our approach, we did this same process for *D. pseudoobscura* which has chromosomal annotation.
We achieved 100% agreement with *D. pseudoobscura* annotation.

#### Classification of evolutionary state

We classified each gene as conserved, gene_death, moved_on, or moved_off based on localization across the genus (**Table 1**).
For example, if a gene is located on Muller D in all species then it is conserved (**Table 1a**).
Similarly, recent_conserved genes were on Muller D in *Sophophora* branch, but else where in *D. virilis* and *D. mojavensis*.
If the gene was on Muller D in all species, but not found in *D. pseudoobscura* then it was considered gene_death.
If the gene was on Muller D in all species, but on another Muller element in *D. pseudoobscura* then it was considered moved_off.
We then furthered grouped genes as selection favoring a gene remaining where it is (conserved, moved_on) or selection against where it is (gene_death, moved_off).

**Table 1a. Classification of genes on Muller element D.**

| Dmel  | Dpse      | (Dwil/Dvir/Dmoj) | Class      | Group               |
|-------|-----------|------------------|------------|---------------------|
| D     | D         | D                | conserved  | Selection Favor D   |
| Other | D         | Other            | moved_on   | Selection Favor D   |
| D     | Other     | D                | moved_off  | Selection Against D |
| D     | Not_found | D                | gene_death | Selection Against D |

**Table 1b. Classification of genes on Muller element E.**

| Dmel  | Dpse      | (Dwil/Dvir/Dmoj) | Class      | Group               |
|-------|-----------|------------------|------------|---------------------|
| E     | E         | E                | conserved  | Selection Favor E   |
| Other | E         | Other            | moved_on   | Selection Favor E   |
| E     | Other     | E                | moved_off  | Selection Against E |
| E     | Not_found | E                | gene_death | Selection Against E |


<!-- TODO: Differential expression analysis. -->
<!-- #### Differential expression -->

### Results

We focused on genes located on Muller elements A (X), D (3L), and E (3R).
The majority of genes were conserved with very few genes moving onto the respective Muller element (**Table 2**).
Gene death in *D. pseudoobscura* maybe overestimated due to quality of gene annotation.

**Table 2. Number of genes per Muller Element.**

|                         | Muller A | Muller D | Muller E |
|-------------------------|----------|----------|----------|
| <b>conserved</b>        | 1,079    | 1,235    | 1,735    |
| <b>recent_conserved</b> | 246      | 337      | 291      |
| <b>moved_on</b>         | 7        | 4        | 4        |
| <b>gene_death</b>       | 375      | 575      | 647      |
| <b>moved_off</b>        | 135      | 158      | 193      |
| <b>other</b>            | 284      | 386      | 468      |

#### Distribution of primary spermatocyte expression

We looked at the proportion of primary spermatocyte cells that expressed orthologous genes in each evolutionary class (**Figure 1,2,3**).
The number of genes in the moved_on and moved_off classes are typically very low (< 10) which makes interpretation difficult.
If we group genes into "Selection Favor" and "Selection Against", followed by a Mann-Whitney U test, we see an significant increased spermatocyte expression for genes with "Selection Against" for all three Muller elements.
These differences appear to be driven by conserved genes and genes that undergone gene death.
Interestingly, the percent of spermatocytes expressing conserved genes is lower for Muller A (X) than D or E.

{{< tabs "cyte_dist" >}}
{{< tab "Muller A" >}}

{{<figure src="./2019-07-22_neox_analysis_boxplot_mullerA.svg" width="100%"
caption="<b>Figure 2. Gene movement on/off of Muller element A.</b> ">}}

Selection Favor vs Selection Against: *Mann-Whitney-U p-value: 1.32e-05*

{{< /tab >}}
{{< tab "Muller D" >}}

{{<figure src="./2019-07-22_neox_analysis_boxplot_mullerD.svg" width="100%"
caption="<b>Figure 3. Gene movement on/off of Muller element D.</b>">}}

Selection Favor vs Selection Against: *Mann-Whitney-U p-value: 4.78e-12*

{{< /tab >}}
{{< tab "Muller E" >}}

{{<figure src="./2019-07-22_neox_analysis_boxplot_mullerE.svg" width="100%"
caption="<b>Figure 4. Gene movement on/off of Muller element D.</b>">}}

Selection Favor vs Selection Against: *Mann-Whitney-U p-value: 3.75e-17*

{{< /tab >}}
{{< /tabs >}}

#### Enrichment of primary spermatocyte biased genes

To how genes important for spermatogenesis behave we looked at differential expression between Spermatogonia and Primary Spermatoctyes.
Below are cross tabulated tables showing counts for the different categories: spermatogonia biased gene expression (gonia), primary spermatocyte gene expression (cyte), and not differentially expressed genes (NS).
Again counts are very low for some classes, but I performed a chi-squre test followed by posthoc analysis to identify enrichment or depletion.
To improve cell counts, I aggregated samples to "Cyte Biased" and "Not Biased" and grouped evolutionary classes.
Again there is evidence of a enrichment of genes that underwent gene_death or "Selection Against" for all tested Muller elements.
While all Muller elements were significant, Muller D shows a more extreme pattern.

{{< tabs "crosstabs" >}}
{{< tab "Muller A" >}}

**Table 3. Number of genes with differential expression on Muller A.**

| DEG   | conserved      | moved_on       | gene_death      | moved_off |
|-------|----------------|----------------|-----------------|-----------|
| NS    | 0<sup>\*</sup> | 1<sup>†</sup>  | 4               | 1         |
| cyte  | 1<sup>\*</sup> | 1              | 9<sup>†</sup>   | 1         |
| gonia | 62<sup>†</sup> | 0<sup>\*</sup> | 19<sup>\*</sup> | 4         |

<span style="font-size: .8em;">𝛘^2: 35.4513, p-value: 0.0000, df: 6</span><br>
<span style="font-size: .8em;"><sup>\*</sup>Significantly depleted (p-value ≤ 0.01)</span><br>
<span style="font-size: .8em;"><sup>†</sup>Significantly enriched (p-value ≤ 0.01)</span>

**Table 4. Number of genes with selection on Muller A.**

| DEG         | Selection Favor | Selection Against |
|-------------|-----------------|-------------------|
| Cyte Biased | 2               | 10                |
| Not Biased  | 63              | 28                |

<span style="font-size: .8em;">Fisher's exact test p-value: 0.0007</span>

{{< /tab >}}
{{< tab "Muller D" >}}

**Table 5. Number of genes with differential expression on Muller D.**

| DEG   | conserved       | gene_death     | moved_off |
|-------|-----------------|----------------|-----------|
| NS    | 8               | 7              | 1         |
| cyte  | 17<sup>\*</sup> | 27<sup>†</sup> | 7         |
| gonia | 86<sup>†</sup>  | 7<sup>\*</sup> | 4         |

<span style="font-size: .8em;">𝛘^2: 50.6224, p-value: 0.0000, df: 4</span><br>
<span style="font-size: .8em;"><sup>\*</sup>Significantly depleted (p-value ≤ 0.01)</span><br>
<span style="font-size: .8em;"><sup>†</sup>Significantly enriched (p-value ≤ 0.01)

**Table 6. Number of genes with selection on Muller D.**

| DEG         | Selection Favor | Selection Against |
|-------------|-----------------|-------------------|
| Cyte Biased | 17              | 34                |
| Not Biased  | 94              | 19                |

<span style="font-size: .8em;">Fisher's exact test p-value: 6.461e-10</span>

{{< /tab >}}
{{< tab "Muller E" >}}

**Table 7. Number of genes with differential expression on Muller E.**

| DEG   | conserved       | moved_on | gene_death      | moved_off |
|-------|-----------------|----------|-----------------|-----------|
| NS    | 13              | 0        | 5               | 2         |
| cyte  | 28<sup>\*</sup> | 1        | 28<sup>†</sup>  | 10        |
| gonia | 117<sup>†</sup> | 0        | 12<sup>\*</sup> | 11        |

<span style="font-size: .8em;">𝛘^2: 41.5756, p-value: 0.0000, df: 6</span><br>
<span style="font-size: .8em;"><sup>\*</sup>Significantly depleted (p-value ≤ 0.01)</span><br>
<span style="font-size: .8em;"><sup>†</sup>Significantly enriched (p-value ≤ 0.01)</span><br>

**Table 8. Number of genes with selection on Muller E.**

| DEG         | Selection Favor | Selection Against |
|-------------|-----------------|-------------------|
| Cyte Biased | 29              | 38                |
| Not Biased  | 130             | 30                |

<span style="font-size: .8em;">Fisher's exact test p-value: 3.264e-08</span>

{{< /tab >}}
{{< /tabs >}}