## Analysis of fecal samples obtained from mice subjected to different diets

### Define objects which will be useful for subsetting data throughout this analysis
The metadata can be extracted as a data frame from the taxonomic SummarizedExperiment object contained within the list called _expvec_. Let's call the metadata dataframe object _pheno_.

```R
pheno <- as.data.frame(colData(expvec$LKT))
```

Now that we have these useful objects, we can proceed to actual plotting.

### Code for Figure 3 panel E
```R
pdf("Fig_3_E.pdf", paper = "a4r")

plot_Ordination(ExpObj = expvec[["LKT"]], algorithm = "PCA", distmethod = "bray",
      compareby = "Diet", colourby = "Diet", shapeby = "Treatment", dotsize = 1.4,
      plotcentroids = TRUE, cdict = cdict, grid = FALSE, forceaspectratio = 1)

dev.off()
```

[Supplementary Fig 3 E pdf output](../pdfs/Fig_3_E.pdf)
