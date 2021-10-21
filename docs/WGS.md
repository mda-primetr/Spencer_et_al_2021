### Code used for shotgun sequencing analysis of microbiome samples of the Spencer et al (2021) paper

For shotgun sequencing analysis of the microbiome samples, the [JAMS package](https://github.com/johnmcculloch/JAMS_BW) was used for both analysis *within* and *between* samples. Although the JAMS package is a *single* software package written mostly in R, the two phases of shotgun sequencing analyis, namely, analysis **within** a sample and comparison **between** samples is run independently in JAMS.

 

### Pre-analysis of samples starting from fastqs.

```{bash eval=FALSE, include=TRUE}
JAMSalpha -f /path/to/forward/Sample_R1.fastq -r /path/to/reverse/Sample_R2.fastq -d /path/to/JAMSdb/JAMSdbApr2020_96Gbk2db -A metagenome -p SamplePrefix
```

### Comparison between samples
#### Building an feature table using the output from JAMSalpha

```{bash eval=FALSE, include=TRUE}
JAMSbeta -p Spencer2021 -y /path/to/folder/with/jamsfiles -t metadata_file.tsv
```

This will create an R-session image (.RData) containing [SummarizedExperiment](https://bioconductor.org/packages/release/bioc/html/SummarizedExperiment.html) objects which can be used with the JAMS package plotting functions.


```{R}
library(JAMS)
```
