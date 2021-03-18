# BinaryOptim-GRAMMAR(BinaryOpGR)
# 1. Getting started
## 1.1	Downloading BinaryOpGR
OptimGRAMMAR_binary can be downloaded https://github.com/RunKingProgram/BinaryOptim-GRAMMAR
. It can be installed as a regular R package.
## 1.2	Installing BinaryOpGR
BinaryOpGR links to R packages Rcpp, RcppEigen and RcppArmadillo, and also imports R packages BEDMatrix and data.table. These dependencies should be installed before installing OptimGRAMMAR_binary. In addition, OpGRbinary requires a recompiled PLINK2.0 Software (http://www.cog-genomics.org/plink/2.0/) with name “plink2offset” under your run directory. Here is an example for installing OpGRbinary and all its dependencies in an R session(assuming none of the R packages other than the default has been installed):
```
install.packages(c("BEDMatrix", "data.table", "Rcpp", "RcppEigen", "RcppArmadillo"), repos = "https://cran.r-project.org/")
system(“R CMD INSTALL BinaryOpGR_1.0_R4.0_MacOS.tgz”)
```
# 2. Input
BinaryOpGR requires the phenotype and genotype files in an PLINK BED data frame which also called PLINK 1 binary file and the structure of these files is described in http://www.cog-genomics.org/plink/1.9/formats#bed. How to prepare these data are describe below.
## 2.1 Phenotype
Phenotype should place in the sixth column of “.fam” file. The missing phenotype value for quantitative traits is -9.
## 2.2 Genotypes
OpGRbinary can only take genotype files in “.bed” and “.bim” format.
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
An object class of character: the filename before the suffix of plink files. The three plink file must have a same filename, for example, “filename.bed”, “filename.bim” and “filename.bam”.
#### nsmar 
An optional numeric for the number of sampling markers. If this is unset, the “nsmar” default is 5,000.
#### msampling
logicals. If FLASE, entire markers will be used to calculate GRM. If this is unset or for large scale dataset, the “msampling” default is TURE.
Here we provide two simple examples of data management for GRL using Data.
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
The total number of SNPs used for optimization, which is set by  default at 50000.<br>
#### Test
An optional for association test, a test at once or joint analysis. You can choose “Separate” for a 
            test at once and “Joint” for further joint analysis based on the result of “Separate”.<br>
#### QQ
logicals. If TURE, Q-Q plot would be drawn.<br>
#### Manh
logicals. If TURE, Q-Q plot would be drawn.<br>

# 4.Example

```
library(BinaryOpGR)
library(BEDMatrix)
library(data.table)

setwd("./example")
plinkfilename = "geno"
gdata <- Data(plinkfilename)
grammar <- OptimGRAMMAR_binary(gdata, Test = "Separate" , QQ = F, Manh = F)
```
