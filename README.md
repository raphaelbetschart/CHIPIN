# CHIPIN
ChIP-seq Intersample Normalization

## INTRODUCTION

ChIPIN is a R package that aim to provide a normalization procedure to compare signals between different conditions/samples of a different ChIP-seq experiment using the same antibody. It will output the resulting density profiles. The normalization is based on the assumption that no differences in ChIP-seq signal should be observed in the regulatory regions of genes whom expression does not change across samples/conditions. Moreover, ChIPIN provides a possibility to qualify the efficiency of the antibody used in the ChIP-seq experiment. 


## REQUIREMENT

Some R libraries are required:
stringr
rtracklayer
GenomicFeatures
ggplot2
matrixStats
viridis
preprocessCore
tidyr
plotrix
rlist
RColorBrewer
tiger
pracma
dplyr
gridExtra
biomaRt
rminer
ggpubr




Deeptools is also required.
Please see additional Python libraries required for Deeptools.
https://deeptools.readthedocs.io/en/develop/content/installation.html


## INSTALLATION

After downloading the .zip archive, one should un-zip it and run: 
R CMD INSTALL pathTo/CHIPIN-master

## FUNCTIONS

### plot_expression function: 
CHIPIN offers the possibility to profile ChIP-seq intensity around TSS as a function of gene expression level using the function plot_expression. The signal is visualized for three groups of genes obtained with k-means clustering: highly-expressed, medium-expressed and lowly-expressed genes. The results of such visualization are important to verify the efficiency of the antibody used. To use this function, several parameters are mandatory:

* **RPKM** is the path to RPKM values, each column of the data should correspond to one sample/condition. If one provides raw_read_count data, put NULL for raw_read_count parameter.
* **raw_read_count** is the path to raw read count values, each column of the data should correspond to one sample/condition. If one provides raw_read_count data, put NULL for RPKM parameter.
* **fileWithPaths** is a textfile with lines corresponding to the paths to the bigWig files.
* **output_dir** is the path to the directory where one wants to store all output of the ChIPIN_normalize function. This directory should be created before running the function.
* **organism** is one of “hg19”, “hg38”, “mm9” or “mm10”.

One additional parameter can be modified:

* **histoneMark** (default: “ChIP-seq signal”) will be used for the legend of the figures.

```
plot_expression(RPKM, raw_read_count, fileWithPaths, output_dir, organism, histoneMark="ChIP-seq signal")
```

This function can be also launch directly in the ChIPIN_normalize function by setting the parameter expression_plot to TRUE.



### ChIPIN_normalize function
This is the main function of the package that should be used to find the “ConstantGenes” and to perform the normalization process. All the parameters of this function will be explained in the following sections. There is three parts in the ChIPIN_normalize function:

* Determine ConstantGenes
* Perform normalization
* Compute statistics

Common parameters for the three parts of ChIPIN_normalize function:

* **sample_name** will be used as a prefix for the different parameters.
* **output_dir** is the path to the directory where one wants to store all output of the ChIPIN_normalize function. This directory should be created before running the function.
* **organism** is one of “hg19”, “hg38”, “mm9” or “mm10”.

To determine “ConstantGenes”, the mandatory parameters are:

* **RPKM** is the path to RPKM values, each column of the data should correspond to one sample/condition. If one provides raw_read_count data, put NULL for raw_read_count parameter.
* **raw_read_count** is the path to raw read count values, each column of the data should correspond to one sample/condition. If one provides raw_read_count data, put NULL for RPKM parameter.

Other paramaters can be modified :

* **percentage** (default: 10) corresponds to the percentage of all genes that will be selected as ConstantGenes.
* **pathToFileGenesNotMoving** (default: NULL) is the path to a bed file containing “ConstantGenes”.

To perform normalization, mandatory parameters are:
* **fileWithPaths** is a textfile with lines corresponding to the paths to the bigWig files.

Additional parameters can be modified:

* **beforeRegionStartLength** (default: 4000), **afterRegionStartLength** (default: 4000), **regionBodyLength** (default: 40000), **binSize** (default: 10) are “computematrix” function’s parameters.
* **expression_plot** (default: FALSE) is a boolean parameter, if equal to TRUE the function plot_expression, previously mentionned, will be run.
* **compute_stat** (default: FALSE) is a boolean parameter, if equal to TRUE, CHIPIN will compute statistics illustrating sucess of the normalization process (i. e. the relative difference between average signal curves before and after the normalization).
* **typeNorm** (default: “quantile”) can be “quantile” or “linear”.
* **nGroup** (default: 20) is a parameter dedicated to quantile normalization.
* **histoneMark** (default: “ChIP-seq signal”) is used in the legend of plots.

```
ChIPIN_normalize(RPKM, raw_read_count, pathToFileGenesNotMoving=NULL, sample_name, output_dir, organism, fileWithPaths, beforeRegionStartLength=4000, afterRegionStartLength=4000, regionBodyLength=40000, binSize=10, expression_plot=FALSE, compute_stat=FALSE, percentage=10, typeNorm="quantile", nGroup=20, histoneMark="ChIP-seq signal")
```


## EXAMPLES: 

Examples for running the two functions are provided in the vignette file included in the zip file "CHIPIN-vignette.html". 


