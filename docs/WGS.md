## Code used for shotgun sequencing analysis of microbiome samples of the Spencer et al (2021) paper

For shotgun sequencing analysis of the microbiome samples, the [JAMS package](https://github.com/johnmcculloch/JAMS_BW) was used for both analysis *within* and *between* samples. Although the JAMS package is a *single* software package written mostly in R, the two phases of shotgun sequencing analyis, namely, analysis **within** a sample and comparison **between** samples is run independently in JAMS.

Briefly, fastqs for each sample are first put through a pipeline called [JAMSalpha](https://github.com/johnmcculloch/JAMS_BW/wiki/JAMSalpha), which yields a _single_ file with extension _.jams_ (called a jamsfile).

These jamsfiles (one for each sample) are then used, together with metadata, to obtain features-by-sample matrices through the [JAMSbeta](https://github.com/johnmcculloch/JAMS_BW/wiki/JAMSbeta) pipeline.


### Pre-analysis of samples starting from fastqs.

```bash
JAMSalpha -f /path/to/forward/Sample_R1.fastq -r /path/to/reverse/Sample_R2.fastq -d /path/to/JAMSdb/JAMSdbApr2020_96Gbk2db -A metagenome -p SamplePrefix
```

### Comparison between samples
#### Building a feature table using the outputs from JAMSalpha
The JAMSbeta pipeline will look for sample prefixes present within a column named "Sample" of the metadata file supplied and select the corresponding jamsfiles present within the folder specified following the `-y` argument:

```bash
JAMSbeta -p Spencer2021 -y /path/to/folder/with/jamsfiles -t metadata_file.tsv
```

This will create an R-session image (.RData) containing [SummarizedExperiment](https://bioconductor.org/packages/release/bioc/html/SummarizedExperiment.html) objects which can be used with the JAMS package plotting functions.

On this paper, shotgun metagenome sequencing of fecal samples was used for evaluating three kinds of samples. Click below for the code used and associated figures in each category:

#### [Human samples](./WGSHuman.md)

#### [Samples from mice submitted to different diets](./WGSMouseDiet.md)

#### [Samples from mice exposed to different probiotics](./WGSMouseProbiotics.md)
