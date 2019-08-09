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
To investigate this question we look at gene movement off of the Neo X chromosome in *D. pseudoobscura* and *D. willistoni*.
In this group, the Muller element A (X) fused with Muller element D (3L) (**Figure 1**).
We expect that genes important for spermatogenesis, on Muller element D, will move to another chromosome or be lost.

{{<figure src="drosophila_synteny.png" width="100%"
caption="<b>Figure 1: Chromosome synteny across the Drosophila genus.</b> Image modified from http://flybase.org/maps/synteny.">}}

First, we need to characterize gene movement in the Drosophila genus.
Then we can look for enrichment of gene movement off of Muller D in genes up regulated in primary spermatocytes.
To characterize gene movement we look at gene localization in a subset of the Drosophila genus.
We focused on 5 members of the genus: *D. pseudoobscura*, *D. melanogaster*, *D. willistoni*, *D. mojavensis*, and *D. virilis*.
I downloaded updated gene annotations and ortholog mappings from [GSE99574](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE99574).
I supplemented these ortholog mappings with those provided by
[FlyBase](ftp://ftp.flybase.net/releases/FB2018_05/precomputed_files/orthologs/dmel_orthologs_in_drosophila_species_fb_2018_05.tsv.gz).
We selected a set of 13,473 orthologous genes present in *D. melanogaster* and at least one of the other species.
Using these gene locations we can determine how the gene is behaving in *D. pseudoobscura* and *D. willistoni*.

### Methods

#### Identification of orthologs

The quality of assemblies and annotations are highly variable across the Drosophila genus.
Genes from *D. willistoni*, *D. mojavensis*, and *D. virilis* are currently mapped to scaffolds instead of chromosome arm.
We used annotations from *D. melanogaster* to assign Muller element to each of these scaffolds using a consensus approach.
For each scaffold, with > 20 genes, we looked where the orthologs were located in *D. melanogaster*.
We assigned the Muller element using majority rule.
To validate our approach, we did this same process for *D. pseudoobscura* which has chromosomal annotation.
We achieved 100% agreement with *D. pseudoobscura* annotation.

#### Classification of evolutionary state

We classified each gene as conserved, gene_death, moved_on, or moved_off based on localization across the genus (**Table 1**).
For example, if a gene is located on Muller D in all species then it is conserved (**Table 1a**).
Similarly, recent_conserved genes were on Muller D in *Sophophora* branch, but were elsewhere in *D. virilis* and *D. mojavensis*.
To determine movement in *D pseudoobscura* we did the following.
If the gene was on Muller D in all species, but not found in *D. pseudoobscura* then it was considered gene_death.
If the gene was on Muller D in all species, but on another Muller element in *D. pseudoobscura* then it was considered moved_off.
We used the same logic for *D. willistoni*.
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

### Results

#### Ortholog selection

Yang et al. only generated orthologs based on genes expressed in their data.
There were a number of FlyBase annotated orthologs that were excluded from their list.
The majority of orthologs present in both data sets were mapping to the same Muller element (**Table 2a**).
While the similarity of missing orthologs was very different (**Table 2a**).
I merged these ortholog sets by updating the FlyBase annotation using Yang et al. mappings (**Table 2a**).
The number of genes present on each Muller element is similar across species with most elements having ≤ 175 different (**Table 2b**).

**Table 2a. Similarity of Yang et al. vs FlyBase.**

| species | Num Non Missing | Similarity of Non Missing<sup>†</sup> | Num Yang Missing | Num FlyBase Missing | Similarity of Missing<sup>†</sup> | Number Orthologs |
|---------|-----------------|---------------------------------------|------------------|---------------------|-----------------------------------|------------------|
| dmel    | 12,599          | 1                                     | 961              | 887                 | -0.0573347                        | 13,559           |
| dpse    | 9,533           | 0.987294                              | 4,597            | 2,250               | 0.301446                          | 12,358           |
| dvir    | 9,657           | 0.993625                              | 4,427            | 2,663               | 0.393496                          | 11,948           |
| dmoj    | 9,747           | 0.994893                              | 4,316            | 2,772               | 0.427417                          | 11,855           |
| dwil    | 9,751           | 0.987739                              | 4,337            | 2,544               | 0.384694                          | 12,102           |

<sup>†</sup>[Adj. Rand Score](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.adjusted_rand_score.html).

**Table 2b. Number of genes per muller element.**

| Species     | A      | B      | C      | D      | E      | F     | Unknown Scaffold |
|-------------|--------|--------|--------|--------|--------|-------|------------------|
| dmel        | 2,102  | 2,582  | 2,766  | 2,656  | 3,351  | 78    | 24               |
| dpse        | 1,850  | 2,281  | 2,458  | 2,310  | 3,087  | 0     | 372              |
| dvir        | 1,913  | 2,184  | 2,419  | 2,316  | 2,930  | 77    | 109              |
| dmoj        | 1,858  | 2,222  | 2,398  | 2,262  | 2,970  | 93    | 52               |
| dwil        | 1,984  | 2,175  | 2,489  | 2,219  | 3,075  | 0     | 160              |
| **Std Dev** | 104.55 | 169.13 | 149.52 | 174.11 | 164.38 | 45.72 | 138.14           |

#### Gene movement classification

For gene movement results browse to the respective page either using the links below or the side bar.

**Table 3. *D. pseudoobscura* movement classes.**

|                  | muller_A   | muller_D   | muller_E   |
|------------------|------------|------------|------------|
| conserved        | 1,571      | 1,919      | 2,557      |
| recent_conserved | 140        | 159        | 214        |
| moved_on         | 21         | 10         | 19         |
| gene_death       | 245        | 373        | 390        |
| moved_off        | 64         | 55         | 44         |
| other            | 247        | 351        | 433        |

**Table 4. *D. willistoni* movement classes.**

|                  | muller_A   | muller_D   | muller_E   |
|------------------|------------|------------|------------|
| conserved        | 1,571      | 1,919      | 2,557      |
| recent_conserved | 140        | 159        | 214        |
| moved_on         | 0          | 0          | 0          |
| gene_death       | 256        | 388        | 487        |
| moved_off        | 32         | 141        | 81         |
| other            | 289        | 260        | 318        |

#### Gene movement cell type enrichment

##### D. pseudoobscura

- [SP vs Primary Spermatocytes]({{<relref "dpse_gonia_vs_cytes.md">}})
- [SP vs EPS]({{<relref "dpse_gonia_vs_eps.md">}})
- [SP vs PS]({{<relref "dpse_gonia_vs_ps.md">}})
- [EPS vs PS]({{<relref "dpse_eps_vs_ps.md">}})

##### D. willistoni

- [SP vs Primary Spermatocytes]({{<relref "dwil_gonia_vs_cytes.md">}})
- [SP vs EPS]({{<relref "dwil_gonia_vs_eps.md">}})
- [SP vs PS]({{<relref "dwil_gonia_vs_ps.md">}})
- [EPS vs PS]({{<relref "dwil_eps_vs_ps.md">}})
