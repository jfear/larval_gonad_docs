---
title: "Clustering"
weight: 2
# bookFlatSection: false
# bookShowToC: true
---

## Larval Testis Clustering

Here we cluster cells based on their expression profiles.
We will use Seurat v3 to perform normalization dimensionality reduction and clustering for each replicate individually.
We will also combine replicates using a canonical correlation analysis approach and cluster cells together.

The integrated analysis can be viewed [here]({{< relref "combined_n3.md" >}}).