---
title: "Intronless Gene Expression"
weight: 3
# bookFlatSection: false
# bookShowToC: true
---

<span style="font-size: 3em; font-weight: 'strong'; color: #ff0000;">WARNING: Preliminary Analysis</span>

## Intronless Gene Expression

Here I look at intronless genes in primary spermatocytes.
The use of intronless genes are important in two areas:
(1) new genes from retro-transposition are likely to have no introns,
(2) genes without introns are easier to transcribe.

### How are intronless genes expressed in primary spermatocytes?

{{<figure src="./x_escapers_and_intronless_genes.svg" width="100%"
caption="<b>Figure 1. Percent of spermatocyte cells with expression of intronless genes by chromosome arm.</b>">}}

To get a general view of intronless expression, I looked at the percent
of primary spermatocyte cells (EPS, PS1, PS2, PS3) that express intron
containing or intronless genes (**Figure 1**).
Spermatocyte biased genes (top row) are expressed in a higher percentage of spermatocyte cells.
While intronless genes are expressed in fewer cells than intron containing genes irrespective of chromosome arm.
However, the number of intronless genes are up regulated in spermatocytes is higher than expected (**Table 1**).
The overall enrichment of intronless genes in spermatocytes can be see by looking by looking at expression patterns (**Figure 2**).

**Table 1. Spermatocyte biased expressed genes are enriched for intronless genes.**

| intronless | NS              | cyte             | gonia           |
|------------|-----------------|------------------|-----------------|
| False      | 64<sup>\*</sup> | 219<sup>\*</sup> | 586<sup>†</sup> |
| True       | 44<sup>†</sup>  | 141<sup>†</sup>  | 65<sup>\*</sup> |

<span>𝛘^2: 137.1037, p-value: 0.0000, df: 2</span><br>
<span><sup>\*</sup>Depleted: Fewer genes than expected.</span><br>
<span><sup>†</sup>Enriched: More genes than expected.</span>

{{<figure src="./x_escapers_and_intronless_genes_heatmap.svg" width="100%"
caption="<b>Figure 2. Intronless gene expression patterns.</b> Z-scores of TPM normalized expression for 2,768 intronless genes.">}}

### Are intronless genes enriched in X chromosome escapers?

Next were are interested to see the relationships among X chromosome, spermatocyte biased gene expression, and intronless genes.
This question requires statistical modeling because the X chromosome is already depleted of intronless genes (**Table 2**).
We used logistic regression to ask if spermatocyte bias and X chromosome status could predict if a gene was intronless.
First, we looked at the main effects model (**Figure 3**).
Similar to above results, we see that genes with spermatocyte bias expression have an increased likelihood of being intronless.
However, we do not see an effect of X chromosome localization independent of spermatocyte expression.
If we then add the interaction term (**Figure 4**), we see a significant decrease in likelihood of intronless genes being spermatocyte biased and on the X.

**Table 2. The X chromosome is depleted for intronless.**

| intronless | 2L                 | 2R    | 3L    | 3R                 | 4               | X                 | Y               |
|------------|--------------------|-------|-------|--------------------|-----------------|-------------------|-----------------|
| False      | 2,406<sup>\*</sup> | 2,604 | 2,490 | 3,183<sup>†</sup>  | 96<sup>†</sup>  | 2,036<sup>†</sup> | 34<sup>\*</sup> |
| True       | 1,095<sup>†</sup>  | 1,024 | 974   | 1,018<sup>\*</sup> | 15<sup>\*</sup> | 640<sup>\*</sup>  | 79<sup>†</sup>  |

<span>𝛘^2: 179.5973, p-value: 0.0000, df: 6</span><br>
<span><sup>\*</sup>Depleted: Fewer genes than expected.</span><br>
<span><sup>†</sup>Enriched: More genes than expected.</span>

{{<figure src="./x_escapers_and_intronless_genes_main_effects.png" width="100%"
caption="<b>Figure 3. Logistic regression results of the main effects model.</b> Logit(intronless = cyte_biased + X chromosome)">}}

{{<figure src="./x_escapers_and_intronless_genes_full.png" width="100%"
caption="<b>Figure 4. Logistic regression of the full model.</b> Logit(intronless = cyte_biased + X chromosome + cyte_biased * X chromosome)">}}
