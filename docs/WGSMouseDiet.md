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

[Fig 3 E pdf output](../pdfs/Fig_3_E.pdf)



### Code for Supplementary Figure 14 panel B
```R
pdf("Supplementary_Fig_S14_B.pdf", paper = "a4r")

plot_relabund_heatmap(ExpObj = expvec[["LKT"]], hmtype = "comparative", compareby = "Diet", invertbinaryorder = TRUE, splitcolsby = NULL, colcategories = c("Vendor", "Diet", "Treatment"), secondaryheatmap = NULL, applyfilters = "moderate", fun_for_l2fc = "geom_mean", showonlypbelow = 0.05, minl2fc = 1.5, adjustpval = TRUE, scaled = FALSE, cdict = cdict, class_to_ignore = "N_A", no_underscores = TRUE)

dev.off()
```
[Supplementary Fig 14 B pdf output](../pdfs/Supplementary_Fig_S14_B.pdf)


### Code for Supplementary Figure 14 panel C
```R
pdf("Supplementary_Fig_S14_C.pdf", paper = "a4r")

plot_relabund_heatmap(ExpObj = expvec[["GO"]], hmtype = "comparative", compareby = "Diet", invertbinaryorder = TRUE, splitcolsby = NULL, colcategories = c("Vendor", "Diet", "Treatment"), secondaryheatmap = NULL, applyfilters = NULL, featcutoff = c(70, 5), minl2fc = 2, fun_for_l2fc = "geom_mean", showonlypbelow = 0.05, adjustpval = TRUE, scaled = FALSE, cdict = cdict, class_to_ignore = "N_A", no_underscores = TRUE)

dev.off()
```
[Supplementary Fig 14 C pdf output](../pdfs/Supplementary_Fig_S14_C.pdf)
