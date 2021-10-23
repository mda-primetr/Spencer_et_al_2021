## Analysis of fecal samples obtained from humans

### Define objects which will be useful for subsetting data throughout this analysis
The metadata can be extracted as a data frame from the taxonomic SummarizedExperiment object contained within the list called _expvec_. Let's call the metadata dataframe object _pheno_.
From this, it is easy to create character vectors with the names of samples contained in subsets of the data used on the paper. These vectors can be passed to any plotting function in JAMS through the _samplesToKeep_ argument, so that the function will only take these explicit samples into consideration.

```R

pheno <- as.data.frame(colData(expvec$LKT))

#Vector of all samples, n=167
Samples_All <- rownames(pheno)

#Vector of all samples, n=133
Samples_antiPD1 <- subset(pheno, treatment == "anti-PD1")[]$Sample

#Validation samples were defined as anti-PD1 treatment which were not in the previous Science2018 cohort, n=111
Samples_Validation <- pheno$Sample[intersect(which(pheno$treatment == "anti-PD1"), which(pheno$sci2018 == "N_A"))]

```

Now that we have these useful objects, we can proceed to actual plotting.

### Code for Supplementary Figure 2 panel A

```R
# Code for Supplementary Figure 2 panel A

pdf("Supplementary_Fig_S2A.pdf", paper = "a4r")

plot_relabund_features(ExpObj = expvec$LKT, glomby = "Genus", samplesToKeep = Samples_Validation,
    featuresToKeep = "g__Faecalibacterium", compareby = "response", fillby = "response",
    applyfilters = "moderate", cdict = cdict)

plot_relabund_features(ExpObj = expvec$LKT, glomby = "Family", samplesToKeep = Samples_Validation,
    featuresToKeep = "f__Ruminococcaceae", compareby = "response", fillby = "response",
    applyfilters = "moderate", cdict = cdict)

dev.off()

```

[Supplementary Figure 2 A pdf output](../pdfs/Supplementary_Fig_S2A.pdf)


### Code for Supplementary Figure 2 panels C and D
For plotting the volcano plots on Supplementary figure 2 panels C and D, we can use a _for_ loop to generate two volcano plots, one for all samples (C) and one only for anti-PD1 samples (D)

```R

pdf("Supplementary_Fig_S2_C_and_D.pdf", paper = "a4r")
for (Wanted_Samples_object_name in c("Samples_All", "Samples_antiPD1")){

    #Create an empty list to hold the stats of difference between clinical response at each taxonomic level.
    Volcano_Stats_list <- list()

    #We will use the plot_relabund_heatmap function of JAMS to return a dataframe with the stats for comparison
    #within each group by setting the argument returnstats=TRUE. As as the function produces heatmaps, we might as well
    #dump them into the pdf, although these heatmaps were not included on the paper.

    #loop through each taxonomic level
    for (glmby in c("LKT", "Genus", "Family", "Class", "Order", "Phylum")){

        tmpstat <- plot_relabund_heatmap(ExpObj = expvec$LKT, glomby = glmby, hmtype = "comparative", samplesToKeep = get(Wanted_Samples_object_name), featuresToKeep = NULL, subsetby = NULL, compareby = "response", invertbinaryorder = TRUE, splitcolsby = "response", colcategories = "response", secondaryheatmap = NULL, applyfilters = "moderate", minl2fc = 0.2, fun_for_l2fc = "geom_mean", showonlypbelow = 0.5, scaled = TRUE, cdict = NULL, returnstats = TRUE, class_to_ignore = "N_A", no_underscores = TRUE)

        tmpstat <- tmpstat[[1]]
        Volcano_Stats_list[[glmby]] <- tmpstat

    }

    #Join all data frames with stats (one for each taxonomic level) into a single data frame
    Volcano_Stats <- bind_rows(Volcano_Stats_list, .id = "Taxonomic_level")
    Volcano_Stats$Taxon <- rownames(Volcano_Stats)
    Volcano_Stats <- Volcano_Stats[ , c("Taxon", "Taxonomic_level", "pval", "padj_fdr", "l2fc", "absl2fc", "MedianGenomeComp", "SDGenomeComp")]

    #Make taxon names clearer, as JAMS deals with "Last Known Taxon" at its most granular level, so LKT__g__Clostridium means
    #"a species, which is unknown at the species level, but belongs to the Clostridium genus". This is not to be confused with
    #the feature g__Clostridium, which is the sum of all LKT features which belong to the genus Clostridium.

    Volcano_Stats$Taxon[which(Volcano_Stats$Taxon == "LKT__g__Roseburia")] <- "LKT__Unclassified_Roseburia_bacterium"
    Volcano_Stats$Taxon[which(Volcano_Stats$Taxon == "LKT__g__Clostridium")] <- "LKT__Unclassified_Clostridium_bacterium"
    Volcano_Stats$Taxon[which(Volcano_Stats$Taxon == "LKT__g__Ruminococcus")] <- "LKT__Unclassified_Ruminococcus_bacterium"
    Volcano_Stats$Taxon[which(Volcano_Stats$Taxon == "LKT__g__Parabacteroides")] <- "LKT__Unclassified_Parabacteroides_bacterium"
    Volcano_Stats$Taxon[which(Volcano_Stats$Taxon == "LKT__g__Faecalibacterium")] <- "LKT__Unclassified_Faecalibacterium_bacterium"

    Volcano_Stats$diffexpressed <- "NO"
    Volcano_Stats$diffexpressed[Volcano_Stats$l2fc > log2(1.5) & Volcano_Stats$padj_fdr <= 0.1] <- "UP"
    Volcano_Stats$diffexpressed[Volcano_Stats$l2fc < -log2(1.5) & Volcano_Stats$padj_fdr <= 0.1] <- "DOWN"

    Volcano_Stats$delabel <- NA
    Volcano_Stats$delabel[Volcano_Stats$diffexpressed != "NO"] <- Volcano_Stats$Taxon[Volcano_Stats$diffexpressed != "NO"]

    Volcano_Stats$Median_Genome_Completeness <- Volcano_Stats$MedianGenomeComp
    Volcano_Stats$Median_Genome_Completeness[Volcano_Stats$Median_Genome_Completeness >= 1] <- 1

    library(ggrepel)

    taxcolours <- c("blue3", "green4", "red2", "cadetblue3", "orange", "black")
    names(taxcolours) <- c("LKT", "Genus", "Family", "Class", "Order", "Phylum")

    pgt5pct <- ggplot(data = Volcano_Stats[Volcano_Stats$MedianGenomeComp >= 0.05, ], aes(x = l2fc, y=-log10(padj_fdr), col = Taxonomic_level, label = delabel)) + geom_point()
    pgt5pct <- pgt5pct + geom_vline(xintercept = c(-log2(1.5), log2(1.5)), col = "grey", linetype = "88") + geom_hline(yintercept=-log10(0.1), col = "grey", linetype = "88")
    pgt5pct <- pgt5pct + scale_colour_manual(values = taxcolours)
    pgt5pct <- pgt5pct + theme_minimal() + geom_text_repel(size = 1.65, min.segment.length = 0)

    #infer the title
    tit <- paste(Wanted_Samples_object_name, paste0(sapply(1:length(table(pheno[get(Wanted_Samples_object_name), "response"])), function(x) { paste(names(table(pheno[get(Wanted_Samples_object_name), "response"]))[x], table(pheno[get(Wanted_Samples_object_name), "response"])[x], sep = "=") }), collapse = " | "))

    pgt5pct <- pgt5pct + ggtitle(tit)

    print(pgt5pct)
}
dev.off()

```
[Supplementary Fig S2 C and D pdf output](../pdfs/Supplementary_Fig_S2_C_and_D.pdf)




### Code for Supplementary Figure 2 panel E
This will plot the Inverse Simpson index for each sample for Responders and Non-Responders within the validation cohort of 111 patients.

```R
pdf("Supplementary_Fig_S2_E.pdf", paper = "a4r")

    print(plot_alpha_diversity(ExpObj = expvec[["LKT"]], measures = "InvSimpson", stratify_by_kingdoms = TRUE,
          samplesToKeep = Samples_Validation, compareby = "response", fillby = "response", applyfilters = "light",
          cdict = cdict))

dev.off()
```
[Supplementary Fig S2 E pdf output](../pdfs/Supplementary_Fig_S2_E.pdf)



### Code for Supplementary Figure 2 panel E
This will yield an ordination plot using UMAP coloured in by Responders and Non-Responders within all 167 samples, and calculating the PERMANOVA value between these groups.

```R
pdf("Supplementary_Fig_S2_F.pdf", paper = "a4r")
for (pcc in c(FALSE, TRUE)){
    print(plot_Ordination(ExpObj = expvec[["LKT"]], algorithm = "tUMAP", distmethod = "bray", compareby = "response",
          colourby = "response", ellipseby = NULL, sizeby = NULL, pairby = NULL, textby = NULL, dotsize = 1.3,
          dotborder = NULL, log2tran = TRUE, transp = TRUE, perplx = NULL, max_neighbors = 15, permanova = TRUE,
          plotcentroids = pcc, cdict = cdict, grid = FALSE, forceaspectratio = 1))
}
dev.off()
```
