---
title: "Intronless Gene Expression"
weight: 3
# bookFlatSection: false
# bookShowToC: true
---

<span style="font-size: 3em; font-weight: 'strong'; color: #ff0000;">WARNING: Preliminary Analysis</span>

## Intronless Gene Expression

### How are intronless genes expressed in primary spermatocytes?

#### Intronless genes are expressed in fewer cells than genes with introns

{{<figure src="./x_escapers_and_intronless_genes.svg" width="100%"
caption="<b>Figure 1. Percent of spermatocyte cells with expression</b>">}}

#### However, intronless genes are enriched in genes with spermatocyte biased expression

| intronless   | type             |      NS |     cyte |    gonia |
|--------------|------------------|---------|----------|----------|
| False        | observed         | 64      | 219      | 586      |
| False        | adj std residual | -4.8295 |  -9.3059 |  11.7041 |
| False        | flag_sig         |  1      |   1      |   1      |
| True         | observed         | 44      | 141      |  65      |
| True         | adj std residual |  4.8295 |   9.3059 | -11.7041 |
| True         | flag_sig         |  1      |   1      |   1      |

𝛘^2: 137.1037, p-value: 0.0000, df: 2

{{<figure src="./x_escapers_and_intronless_genes_heatmap.svg" width="100%"
caption="<b>Figure 2. Genes without introns have increased expression in primary spermatocytes.</b>">}}


### Are intronless genes enriched in X chromosome escapers?

#### Intronless genes are depleted on the X chromosome.

| intronless   | type             |        2L |        2R |        3L |        3R |       4 |         X |       Y |
|--------------|------------------|-----------|-----------|-----------|-----------|---------|-----------|---------|
| False        | observed         | 2406      | 2604      | 2490      | 3183      | 96      | 2036      |  34     |
| False        | adj std residual |   -5.7701 |   -1.2768 |   -1.0827 |    5.2429 |  3.2871 |    4.3642 | -10.171 |
| False        | flag_sig         |    1      |    0      |    0      |    1      |  1      |    1      |   1     |
| True         | observed         | 1095      | 1024      |  974      | 1018      | 15      |  640      |  79     |
| True         | adj std residual |    5.7701 |    1.2768 |    1.0827 |   -5.2429 | -3.2871 |   -4.3642 |  10.171 |
| True         | flag_sig         |    1      |    0      |    0      |    1      |  1      |    1      |   1     |

𝛘^2: 179.5973, p-value: 0.0000, df: 6


#### X chromosome escapers are not enriched for intronless genes.

##### Main Effects Model Logit(intronless = cyte_biased + X chromosome)

{{<figure src="./x_escapers_and_intronless_genes_main_effects.png" width="100%"
caption="<b>Figure 3. Main effects model.</b>">}}

##### Full Model Logit(intronless = cyte_biased + X chromosome + cyte_biased * X chromosome)

{{<figure src="./x_escapers_and_intronless_genes_full.png" width="100%"
caption="<b>Figure 3. Full model.</b>">}}
