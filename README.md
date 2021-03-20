# BinaryOptim-GRAMMAR(BinaryOpGR)
# 1. Getting started
## 1.1	Downloading BinaryOpGR
BinaryOpGR can be downloaded https://github.com/RunKingProgram/BinaryOptim-GRAMMAR
. It can be installed as a regular R package.
## 1.2	Installing BinaryOpGR
BinaryOpGR links to R packages Rcpp, RcppEigen and RcppArmadillo, and also imports R packages BEDMatrix and data.table. These dependencies should be installed before installing BinaryOpGR. In addition, OpGRbinary requires a recompiled PLINK2.0 Software with name “plink2ebv” under your run directory. We have tested HiRRM in CentOS 7, CentOS 7.5 ,Ubuntu 20 and Mac OS 10.15. Here is an example for installing BinaryOpGR and all its dependencies in an R session under Linux，MacOS or Windows Subsystem for Linux:
```
install.packages(c("BEDMatrix", "data.table", "Rcpp", "RcppEigen", "RcppArmadillo"), repos = "https://cran.r-project.org/")
system(“R CMD INSTALL BinaryOpGR_1.0_R4.0_MacOS.tgz”)
```
# 2. Input
## 2.1 Phenotype
Phenotype should place in the sixth column of “.fam” file. The missing phenotype value for quantitative traits is -9.
## 2.2 Genotype
Genotype file consists of three PLINK BED files with the same name. For example, Genotype.bed, Genotype.bim and Genotype.fam.
# 3. Running BinaryOpGR
If OpGRbinary has been successfully installed, you can load it in an R session using:
```
library(OpGRbinary)
```
and then loading "BEDMatrix "and " data.table " 
```
library(BEDMatrix)
library(data.table)
```
We provide two functions in OpGRbinary: **Data** function for basic data management including extract phenotypic values, sampling markers, calculate allele frequencies and calculating GRM, saved in an external file, used for GBLUP method or the transformed GRM for large scale dataset and **OptimGRAMMAR_binary** function for association tests using OpGRbinary method, joint analysis based on the results of OpGRbinary, and outputting association result file then drawing Q-Q and Manhattan plot.
## 3.1 Data function
#### Usage
```
Data (filename, nsmar, msampling)
```
#### Arguments

#### filename
An object class of character，which consists of three PLINK BED files with the same name. For example, Genotype.bed, Genotype.bim and Genotype.fam.
#### nsmar 
An optional numeric for the number of sampling markers. If this is unset, the “nsmar” default is 5,000.
#### msampling
logicals. If FLASE, entire markers will be used to calculate GRM. If this is unset or for large scale dataset, the “msampling” default is TURE.
## 3.2 OptimGRAMMAR_binary function
#### Usage
```
OptimGRAMMAR_binary(Data,maxh2,opsm, Test = c("Joint","Separate") ,QQ=T,Manh=T)
```
#### Arguments
#### Data
An object class of list from the 1st step.<br>
#### maxh2
Upper limit of polygenic heritability, which is set by default at 0.5.<br>
#### opsm
The total number of SNPs used for optimization, which is set by default at 50000.<br>
#### Test
An optional for association test, a test at once or joint analysis.<br>
#### QQ
logicals. If TURE, Q-Q plot would be drawn.<br>
#### Manh
logicals. If TURE, Q-Q plot would be drawn.<br>

# 4.Output files


There are the two outputs from the BinaryOpGR: estimated breeding values（ebv.txt）and Association tests(result.PHENO1.glm.logistic). For example, result.PHENO1.glm.logistic:
CHROM|	POS|	ID|	REF|	ALT|	A1|	TEST|	OBS_CT|	OR|	SE|	Z_STAT|	P|	ERRCODE
---- | ----- | ------ | ------| ------| ------| ------| ------| ------| ------| ------| ------| ------
1|	10390|	S1_10390	|G|	A|	A|	ADD|	2648|	-0.00784112|	0.238845|	-0.0328293	|0.973813|	|.
1|	10590|	S1_10590	|T|	A|	A|	ADD|	2648|	-0.202364|	0.249746|	-0.810281	|0.417852|	|.
1|	128373|	S1_128373|	C|	T|	T|	ADD|	2648|	0.033819|	0.124565|	0.271498	|0.78603|	|.
…
# 5.Example

```
library(BinaryOpGR)
library(BEDMatrix)
library(data.table)

setwd("./example")
plinkfilename = "geno"
gdata <- Data(plinkfilename)
grammar <- OptimGRAMMAR_binary(gdata, Test = "Separate")
```
