## Analysis of fecal samples obtained from mice subjected to different probiotics


### Define objects which will be useful for subsetting data throughout this analysis
The metadata can be extracted as a data frame from the taxonomic SummarizedExperiment object contained within the list called _expvec_. Let's call the metadata dataframe object _pheno_.

```R
pheno <- as.data.frame(colData(expvec$LKT))
Samples_Pre_Treatment <- subset(pheno, Timepoint == "Pre-treatment")[]$Sample
Samples_Post_Treatment <- subset(pheno, Timepoint == "Post-treatment")[]$Sample

```

Now that we have these useful objects, we can proceed to actual plotting.


### Code for Figure 2 panel D

```R
pdf("Fig_2_D.pdf", paper = "a4r")

print(plot_alpha_diversity(ExpObj = expvec[["LKT"]], measures = "InvSimpson",
      stratify_by_kingdoms = TRUE, samplesToKeep = Samples_Poop4, compareby = "Probiotic",
      compareby_order = c("Water", "Probiotic 1 (Bifidobacterium) ", "Probiotic 2 (Lactobacillus) "),
      colourby = NULL, applyfilters = "light", cdict = cdict, class_to_ignore = "N_A"))

dev.off()
```

[Fig 2 D pdf output](../pdfs/Fig_2_D.pdf)



### Code for Figure 2 panel E

```R
pdf("Fig_2_E.pdf", paper = "a4r")

print(plot_Ordination(ExpObj = expvec[["LKT"]], samplesToKeep = NULL, algorithm = "tUMAP",
      distmethod = "bray", compareby = "Probiotic", colourby = "Probiotic", pairby = NULL,
      textby = NULL, dotsize = 1.3, plotcentroids = TRUE, cdict = cdict, grid = FALSE,
      forceaspectratio = 1))

dev.off()
```

[Fig 2 E pdf output](../pdfs/Fig_2_E.pdf)


### Code for Supplementary Figure 9 panel A

```R
pdf("Supplementary_Fig_S9_A.pdf", paper = "a4r")

print(plot_Ordination(ExpObj = expvec[["GO"]], samplesToKeep = Samples_Post_Treatment, algorithm = "tUMAP",
      distmethod = "bray", compareby = "Probiotic", colourby = "Probiotic", ellipseby = NULL, sizeby = NULL,
      pairby = NULL, textby = NULL, dotsize = 1.3, dotborder = NULL, log2tran = TRUE, transp = TRUE, perplx = NULL,
      max_neighbors = 15, permanova = TRUE, plotcentroids = TRUE, cdict = cdict, grid = FALSE, forceaspectratio = 1,
      addtit = "Post-Treatment Samples"))

dev.off()
```

[Supplementary Fig 9 A pdf output](../pdfs/Supplementary_Fig_S9_A.pdf)

### Code for Supplementary Figure 9 panel B

```R
pdf("Supplementary_Fig_S9_B.pdf", paper = "a4r")

plot_relabund_heatmap(ExpObj = expvec$GO, hmtype = "comparative", samplesToKeep = Samples_Post_Treatment, featuresToKeep = NULL,
                      subsetby = NULL, compareby = "Probiotic", splitcolsby = "Probiotic", colcategories = "Probiotic",
                      secondaryheatmap = NULL, applyfilters = "moderate", showonlypbelow = NULL, cluster_rows = FALSE,
                      adjustpval = TRUE, featcutoff = NULL, minl2fc = NULL, scaled = TRUE, cdict = cdict, class_to_ignore = "N_A",
                      no_underscores = TRUE, ntop = 50)

dev.off()
```
[Supplementary Fig 9 B pdf output](../pdfs/Supplementary_Fig_S9_B.pdf)
