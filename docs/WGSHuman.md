## Analysis of fecal samples obtained from humans


```R
# Code for Supplementary Figure 2 panel A

#Validation samples were defined as anti-PD1 treatment which were not in the previous Science2018 cohort.
Samples_Validation <- pheno$Sample[intersect(which(pheno$treatment == "anti-PD1"), which(pheno$sci2018 == "N_A"))]

pdf("Supplementary_Fig_S2A.pdf", paper = "a4r")

plot_relabund_features(ExpObj = expvec$LKT, glomby = "Genus", samplesToKeep = Samples_Validation, featuresToKeep = "g__Faecalibacterium", compareby = "response", fillby = "response", applyfilters = "moderate", cdict = cdict)

plot_relabund_features(ExpObj = expvec$LKT, glomby = "Family", samplesToKeep = Samples_Validation, featuresToKeep = "f__Ruminococcaceae", compareby = "response", fillby = "response", applyfilters = "moderate", cdict = cdict)

dev.off()

```

[FigS2A pdf output](../pdfs/Supplementary_Fig_S2A.pdf)
