## Analysis of fecal samples obtained from mice subjected to different probiotics


### Define objects which will be useful for subsetting data throughout this analysis
The metadata can be extracted as a data frame from the taxonomic SummarizedExperiment object contained within the list called _expvec_. Let's call the metadata dataframe object _pheno_.

```R
pheno <- as.data.frame(colData(expvec$LKT))
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
for(algo in c("PCA", "tUMAP")){
    print(plot_Ordination(ExpObj = expvec[["LKT"]], samplesToKeep = NULL, algorithm = algo,
    distmethod = "bray", compareby = "Probiotic", colourby = "Probiotic", pairby = NULL,
    textby = NULL, dotsize = 1.3, plotcentroids = TRUE, cdict = cdict, grid = FALSE,
    forceaspectratio = 1))
}
dev.off()
```

[Fig 2 E pdf output](../pdfs/Fig_2_E.pdf)
