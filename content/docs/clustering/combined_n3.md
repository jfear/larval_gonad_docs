---
title: Integrated Clustering (n = 3)
author: Justin Fear
date: 'Compiled: June 24, 2019'
weight: 1
---

## Integrated Clustering (n = 3; Seurat v3)

After testing both Seurat and Monocle (v2 and v3), I have decided to move forward with Seurat v3.
All method give similar clusters, but Seurat v3 provides more advanced data integration and differential expression testing.
Here I describe the integration of replicates 1, 2, and 3.
This is followed by clustering of cells and annotation of cell type based on expression of known marker genes.

### Individual Replicate Clustering

I started by clustering each replicate separately.
This gives me an idea of overall data structure and a feeling for how many clusters to expect.
Each replicate had very similar clustering patterns, with very distinct germline and somatic clusters.
See individual clustering documentation for more details.

{{<figure src="../combined_n3_figures/dimplots-1.png" width="100%"
caption="**Figure 1. Individual clusters from three replicates (Testis 1, 2, and 3 respectively).**">}}

### Integration and Clustering

Replicates are not common in scRNA-Seq.
We could cluster each replicate separately and then map clusters between replicates.
Or we can combine cells from all replicates and cluster once.
The later would incorporate more data into the clustering callings process, which may improve overall clustering results.

Here we combine replicates and then cluster.
First, we remove replicate batch effects.
To do this, Seurat provides an alignment method based on canonical correlation analysis (CCA) which tries to improve correlation among samples.
They have made some slight improvements over Seurat v2.
I performed CCA in 30 dimensional space and selected the first 9 dimensions to run KNN clustering.
For community detection (Louvain algorithm) I used a resolution of 0.3 which gives 10 clusters.

The 2D projections using uniform manifold approximation and projection (UMAP) shows a similar structure as individual replicates (**Figure 1, 2, 3**).
Each replicate is represented in each cluster (**Figure 2A, 3**) and clusters range from 853 (9) to 3,217 (0) cells (**Table 1**).
The contribution of cells to each cluster is based on the proportion of cells from each replicate.
With clusters (0, 1, 2) having proportionally more cells from replicate 3 than other clusters.

{{<figure src="../combined_n3_figures/umap_reps-1.png" width="100%"
caption="**Figure 2. UMAP projection of integrated clusters.** Each cell (n = 18,792) is plotted in UMAP space. Colors represent either different replciates (panel A) or different cell types after cluster calling using community detection (panel B). Cluster numbering is random.">}}


{{<figure src="../combined_n3_figures/umap_reps_split-1.png" width="100%"
caption="**Figure 3. UMAP projection of integrated clusters split by replicate.** Cells from each replicate are plotted separately in UMAP space. Colors represent different cell types after clustering.">}}

**Table 1. Number of cells per cluster.**

| Cluster | Num. Cells Per Cluster |
|---------|-----------------------:|
| 0       |                  3,217 |
| 1       |                  3,149 |
| 2       |                  2,652 |
| 3       |                  2,388 |
| 4       |                  1,510 |
| 5       |                  1,466 |
| 6       |                  1,401 |
| 7       |                  1,376 |
| 8       |                    870 |
| 9       |                    853 |

{{<figure src="../combined_n3_figures/barplot-cell-per-cluster-number.svg" width="100%"
caption="**Figure 4. Number of Cells per Replicate per Cluster.** The number of cells from each replicate are plotted by cluster.">}}


### Cluster Annotation

To annotate clusters I used a set of 73 marker genes from the literature (**Figure 5**).
Based on patterns of gene expression, we identified 5 germ cell clusters and 5 somatic cell clusters.

* Spermatogonia (SP: cluster 9)
* Early Primary Spermatocytes (EPS: cluster 5)
* Primary Spermatocytes (PS1: cluster 6)
* Primary Spermatocytes (PS2: cluster 2)
* Primary Spermatocytes (PS3: cluster 0)
* Early Cyst Cells (ECY: cluster 4)
* Cyst Cells (CY1: cluster 1)
* Cyst Cells (CY2: cluster 3)
* Terminal Epithelium (TE: cluster 7)
* Pigment Cells (PC: cluster 8)

We are able to separate the germ cell lineage into spermatogonia (SP), early primary spermatocytes (EPS), and later primary spermatocytes (PS1, PS2, PS3).
The ordering of the later primary spermatocytes is not clear.
Similarly, in the somatic cyst cell lineage we identified an early cyst cell (ECY) based on *tj* expression and two later cyst cell types (CY1 and CY2) with CY2 showing the highest expression of *eya*.

{{<figure src="../combined_n3_figures/heatmap-all-lit-genes-w-rep.svg" width="100%"
caption="**Figure 5. Expression patterns of marker genes from the literature by putative cell type.** We calculated z-scores for each gene by replicate by cluster after TPM normalization. We plot 73 marker genes from the literature as rows and cluster * replicate as columns. Each marker gene is known to describe a specific cell type (labeled on the right). We used these expression patterns to annotate and order clusters. White dashed lines separate clusters (columns) and gene groups (rows).">}}

{{<figure src="../combined_n3_figures/umap.svg" width="100%"
caption="**Figure 6. UMAP projection after annotation.** Each cluster was annotated with a putative cell type based on marker gene expression.">}}

To give a sense of the specificity and localization I plot the various marker genes below.
On the first tab is a Zoom view of the genes I focused on when making annotation decisions.
On the second tab (UMAP) I plot all marker genes for a given cell type.
On the third tab (Violin) I plot the cell distribution for each gene in each cluster.

#### Spermatogonia (Cluster 9)

{{< tabs "spermatogoina" >}}
{{< tab "UMAP (Zoom)" >}}
{{<figure src="../combined_n3_figures/vas_bam-1.png" width="100%">}}
{{< /tab >}}
{{< tab "UMAP" >}}
{{<figure src="../combined_n3_figures/heatmap_gonia-2.png" width="100%">}}
{{< /tab >}}
{{< tab "Violin" >}}
{{<figure src="../combined_n3_figures/heatmap_gonia-1.png" width="100%">}}
{{< /tab >}}
{{< /tabs >}}

#### Spermatocytes (Clusters 0, 2, 5, 6)

{{< tabs "spermatocytes" >}}
{{< tab "UMAP (Zoom)" >}}
{{<figure src="../combined_n3_figures/aly_nht_can_soti-1.png" width="100%">}}
{{< /tab >}}
{{< tab "UMAP" >}}
{{<figure src="../combined_n3_figures/heatmap_spermatocytes-2.png" width="100%">}}
{{< /tab >}}
{{< tab "Violin" >}}
{{<figure src="../combined_n3_figures/heatmap_spermatocytes-1.png" width="100%">}}
{{< /tab >}}
{{< /tabs >}}

#### CySC (Clusters 1, 3, 4)

{{< tabs "cysc" >}}
{{< tab "UMAP (Zoom)" >}}
{{<figure src="../combined_n3_figures/tj_eya-1.png" width="100%">}}
{{< /tab >}}
{{< tab "UMAP" >}}
{{<figure src="../combined_n3_figures/heatmap_cysc-2.png" width="100%">}}
{{< /tab >}}
{{< tab "Violin" >}}
{{<figure src="../combined_n3_figures/heatmap_cysc-1.png" width="100%">}}
{{< /tab >}}
{{< /tabs >}}

#### TE (Cluster 7)

{{< tabs "te" >}}
{{< tab "UMAP" >}}
{{<figure src="../combined_n3_figures/heatmap_te-2.png" width="100%">}}
{{< /tab >}}
{{< tab "Violin" >}}
{{<figure src="../combined_n3_figures/heatmap_te-1.png" width="100%">}}
{{< /tab >}}
{{< /tabs >}}

#### Pigmet Cells (Cluster 8)

{{< tabs "pc" >}}
{{< tab "UMAP" >}}
{{<figure src="../combined_n3_figures/heatmap_pc-2.png" width="100%">}}
{{< /tab >}}
{{< tab "Violin" >}}
{{<figure src="../combined_n3_figures/heatmap_pc-1.png" width="100%">}}
{{< /tab >}}
{{< /tabs >}}

#### Hub (??)

{{< tabs "hub" >}}
{{< tab "UMAP" >}}
{{<figure src="../combined_n3_figures/heatmap_hub-2.png" width="100%">}}
{{< /tab >}}
{{< tab "Violin" >}}
{{<figure src="../combined_n3_figures/heatmap_hub-1.png" width="100%">}}
{{< /tab >}}
{{< /tabs >}}

### Differential Expression

Next I look for differentially expressed genes among clusters.
I use a One vs Rest approach to get a list of genes (biomarkers) that are up regulated in each cluster.
Some biomarkers are up regulated in a single cluster (**Figure 7**) while others were up regulated in multiple cluster (**Figure 8**).
Interestingly, some clusters of the germline and somatic cyst cell lineage have very few uniquely up regulated genes (PS2, PS3, and CY1), while the remaining clusters have more than 100.
Perhaps these cells types are intermediates in the developmental program.
Marker genes expressed in multiple cell types tend to be found within the same lineage, with only 367 genes up regulated in the germline and soma.

{{<figure src="../combined_n3_figures/heatmap-unique-biomarkers-w-rep.svg" width="100%"
caption="**Figure 7. Biomarkers uniquely up regulated in a single cluster.** Using differential expression we identified a set of genes that are significantly up regulated in a specific cluster. For each cluster (columns) we plot z-scores of genes that were uniquely up regulated in that cluster (label on the right and the number of genes). Genes are ordered within each gene group by hierarchical clustering. White dashed lines separate clusters (columns) and gene groups (rows).">}}

{{<figure src="../combined_n3_figures/heatmap-multi-biomarkers-w-rep.svg" width="100%"
caption="**Figure 8. Biomarkers up regulated in multiple clusters.** Using differential expression we identified a set of genes that are significantly up regulated in multiple clusters. We group genes that were up regulated in the germ line lineage only (Germ only), in the somatic lineage only (Soma only) and in both germ and somatic cells (Germ and Soma). Within gene group we used hierarchical clustering to sort genes. White dashed lines separate clusters (columns) and gene groups (rows).">}}

#### Biomarkers on the Literature Gene List

Using the biomakers list I looked to see what fraction of genes overlap our literature gene list.
Every putative cell type had at least a few genes from the literature gene list.
CY1 and CY2 had the lowest representation of literature genes on the biomaker list (7% and 19% respectively).
TE and PC had the best representation (100%), but also had the smallest number of genes.
As expected, the EPS cluster had the early tTAF (*aly*, *can*), while the later staged germ cells PS1, PS2, and PS3 had the later stage tTAFs (*dj*, *ocn*, *fzo*).

{{< tabs "Overlap" >}}

{{< tab "SP" >}}

43% Overlap

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th colspan="2">Literature Gene</th>
    </tr>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>False</th>
      <th>True</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2">Biomarker</th>
      <th>False</th>
      <td>16,528</td>
      <td>9</td>
    </tr>
    <tr>
      <th>True</th>
      <td>1,227</td>
      <td>7</td>
    </tr>
  </tbody>
</table>

* Lit Gene In Biomarker List: bam, Rbp4, Phf7, p53, AGO3, Prosalpha6, vas
* Not In Biomarker List: esg, neur, nos, bgcn, spi, Dad, fest, dpr17, tut

{{< /tab >}}

{{< tab "EPS" >}}

43% Overlap

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th colspan="2">Literature Gene</th>
    </tr>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>False</th>
      <th>True</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2">Biomarker</th>
      <th>False</th>
      <td>16,626</td>
      <td>13</td>
    </tr>
    <tr>
      <th>True</th>
      <td>1,122</td>
      <td>10</td>
    </tr>
  </tbody>
</table>

* Lit Gene In Biomarker List: aub, sa, aly, can, Taf12L, tomb, kmg, CG3927, tbrd-1, nht
* Not In Biomarker List: CycA, CycB, Mst87F, bol, fzo, mia, dj, Reepl1, oys, topi, d-cup, bb8, ocn

{{< /tab >}}

{{< tab "PS1" >}}

43% Overlap

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th colspan="2">Literature Gene</th>
    </tr>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>False</th>
      <th>True</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2">Biomarker</th>
      <th>False</th>
      <td>16,058</td>
      <td>13</td>
    </tr>
    <tr>
      <th>True</th>
      <td>1,690</td>
      <td>10</td>
    </tr>
  </tbody>
</table>

* Lit Gene In Biomarker List: CycB, Mst87F, bol, fzo, dj, Reepl1, CG3927, d-cup, bb8, ocn
* Not In Biomarker List: aub, CycA, sa, aly, can, mia, Taf12L, tomb, kmg, oys, topi, tbrd-1, nht
{{< /tab >}}

{{< tab "PS2" >}}

47% Overlap

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th colspan="2">Literature Gene</th>
    </tr>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>False</th>
      <th>True</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2">Biomarker</th>
      <th>False</th>
      <td>16,295</td>
      <td>12</td>
    </tr>
    <tr>
      <th>True</th>
      <td>1,453</td>
      <td>11</td>
    </tr>
  </tbody>
</table>

* Lit Gene In Biomarker List: CycB, Mst87F, bol, fzo, dj, Reepl1, Taf12L, CG3927, d-cup, bb8, ocn
* Not In Biomarker List: aub, CycA, sa, aly, can, mia, tomb, kmg, oys, topi, tbrd-1, nht
{{< /tab >}}

{{< tab "PS3" >}}

21% Overlap

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th colspan="2">Literature Gene</th>
    </tr>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>False</th>
      <th>True</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2">Biomarker</th>
      <th>False</th>
      <td>17,370</td>
      <td>18</td>
    </tr>
    <tr>
      <th>True</th>
      <td>378</td>
      <td>5</td>
    </tr>
  </tbody>
</table>

* Lit Gene In Biomarker List: Mst87F, dj, Taf12L, CG3927, ocn
* Not In Biomarker List: aub, CycA, CycB, sa, aly, bol, can, fzo, mia, Reepl1, tomb, kmg, oys, topi, d-cup, bb8, tbrd-1, nht

{{< /tab >}}

{{< tab "EYC" >}}

36% Overlap

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th colspan="2">Literature Gene</th>
    </tr>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>False</th>
      <th>True</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2">Biomarker</th>
      <th>False</th>
      <td>17,083</td>
      <td>17</td>
    </tr>
    <tr>
      <th>True</th>
      <td>662</td>
      <td>9</td>
    </tr>
  </tbody>
</table>

* Lit Gene In Biomarker List: tj, bnb, ImpL2, vn, Nrt, piwi, Wnt4, fax, kek1
* Not In Biomarker List: eya, EcR, robo2, sev, so, usp, zfh1, fng, gbb, spict, sano, Cht5, Efa6, Nlg3, rdo, puc, br

{{< /tab >}}

{{< tab "CY1" >}}

7% Overlap

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th colspan="2">Literature Gene</th>
    </tr>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>False</th>
      <th>True</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2">Biomarker</th>
      <th>False</th>
      <td>17,495</td>
      <td>24</td>
    </tr>
    <tr>
      <th>True</th>
      <td>250</td>
      <td>2</td>
    </tr>
  </tbody>
</table>

* Lit Gene In Biomarker List: bnb, fax
* Not In Biomarker List: eya, EcR, tj, ImpL2, robo2, sev, so, usp, vn, Nrt, zfh1, piwi, Wnt4, fng, kek1, gbb, spict, sano, Cht5, Efa6, Nlg3, rdo, puc, br

{{< /tab >}}

{{< tab "CY2" >}}

19% Overlap

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th colspan="2">Literature Gene</th>
    </tr>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>False</th>
      <th>True</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2">Biomarker</th>
      <th>False</th>
      <td>16,929</td>
      <td>21</td>
    </tr>
    <tr>
      <th>True</th>
      <td>816</td>
      <td>5</td>
    </tr>
  </tbody>
</table>

* Lit Gene In Biomarker List: bnb, fax, kek1, Nlg3, rdo
* Not In Biomarker List: eya, EcR, tj, ImpL2, robo2, sev, so, usp, vn, Nrt, zfh1, piwi, Wnt4, fng, gbb, spict, sano, Cht5, Efa6, puc, br

{{< /tab >}}

{{< tab "TE" >}}

100% Overlap

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th colspan="2">Literature Gene</th>
    </tr>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>False</th>
      <th>True</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2">Biomarker</th>
      <th>False</th>
      <td>17,147</td>
      <td>0</td>
    </tr>
    <tr>
      <th>True</th>
      <td>618</td>
      <td>6</td>
    </tr>
  </tbody>
</table>

* Lit Gene In Biomarker List: abd-A, Abd-B, cv-2, N, nord, Piezo

{{< /tab >}}

{{< tab "PC" >}}

100% Overlap

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th colspan="2">Literature Gene</th>
    </tr>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>False</th>
      <th>True</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2">Biomarker</th>
      <th>False</th>
      <td>17,054</td>
      <td>0</td>
    </tr>
    <tr>
      <th>True</th>
      <td>715</td>
      <td>2</td>
    </tr>
  </tbody>
</table>

* Lit Gene In Biomarker List: ems, Sox100B
{{< /tab >}}

{{< /tabs >}}

#### Potentially Miss Expressed Literature Genes

Next I wanted to look for genes that appear as a different marker on the literature gene list.
This is complicated to look at because many of the markers are in multiple cell types.
I only include clusters that had marker gene mapping to another cell type.
There are only a few instances of a germline marker gene being found on the biomarker list of a somatic cell type.
It is hard to know if this is because of cell contamination or if the gene is expressed in that cell type.

{{< tabs "missexpressed" >}}

{{< tab "SP" >}}
* aub (ES, MS)
* Taf12L (ES, MS, LS)
{{< /tab >}}

{{< tab "EPS" >}}
* AGO3 (SP)
{{< /tab >}}

{{< tab "CY2" >}}
* cv-2 (TE)
* Dad (SP)
* Prosalpha6 (ES)
{{< /tab >}}

{{< tab "TE" >}}
* ImpL2 (C, EC)
* robo2 (H, C)
* zfh1 (C, EC)
* spi (SP, ES)
* fax (C, EC, MC)
* kek1 (C, EC, MC, LC)
* Dad (SP)
* gbb (H, C, EC)
* Socs36E (H, C)
* puc (C, EC)
* br (C, EC)
{{< /tab >}}

{{< tab "PC" >}}
* abd-A (H, C, EC)
* EcR (C, EC)
* robo2 (H, C)
* vn (C, EC)
* zfh1 (C, EC)
* N (H, C, TE)
* spi (SP, ES)
* fax (C, EC, MC)
* kek1 (C, EC, MC, LC)
* spict (EC)
* Socs36E (H, C)
* puc (C, EC)
* br (C, EC)
{{< /tab >}}

{{< /tabs >}}
*Note abbreviations in parentheses are based on Miriam's literature gene table.*

#### Biomarker Gene Ontology

To get a better understanding of the underlying biological processes driving the differences among clusters I looked at gene ontology.
SP and EPS have genes associated with transcription and translation.
EPS was also significant for chromatin organization.
PS1, PS2, and P3 show genes associated with sperm development.
With genes up regulated in PS3 almost entirely associated with sperm production.
The cyst cell lineage (ECY, CY1, and CY2) show evidence of genes associated with translation.
The TE and PC clusters show genes associated with morphogenesis and growth.

{{< tabs "Gene Ontology" >}}
{{< tab "SP" >}}
{{<figure src="../combined_n3_figures/sp-1.png" width="100%" >}}
{{< /tab >}}

{{< tab "EPS" >}}
{{<figure src="../combined_n3_figures/eps-1.png" width="100%" >}}
{{< /tab >}}

{{< tab "PS1" >}}
{{<figure src="../combined_n3_figures/ps1-1.png" width="100%" >}}
{{< /tab >}}

{{< tab "PS2" >}}
{{<figure src="../combined_n3_figures/ps2-1.png" width="100%" >}}
{{< /tab >}}

{{< tab "PS3" >}}
{{<figure src="../combined_n3_figures/ps3-1.png" width="100%" >}}
{{< /tab >}}

{{< tab "ECY" >}}
{{<figure src="../combined_n3_figures/ecy-1.png" width="100%" >}}
{{< /tab >}}

{{< tab "CY1" >}}
{{<figure src="../combined_n3_figures/cy1-1.png" width="100%" >}}
{{< /tab >}}

{{< tab "CY2" >}}
{{<figure src="../combined_n3_figures/cy2-1.png" width="100%" >}}
{{< /tab >}}

{{< tab "TE" >}}
{{<figure src="../combined_n3_figures/te-1.png" width="100%" >}}
{{< /tab >}}

{{< tab "PC" >}}
{{<figure src="../combined_n3_figures/pc-1.png" width="100%" >}}
{{< /tab >}}

{{< /tabs >}}
