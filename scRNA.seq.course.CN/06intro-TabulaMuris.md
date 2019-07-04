---
output: html_document
---

# Tabula Muris {#tabula_muris}

## 介绍

为了提供完整分析scRNA-seq数据的体验，我们以[Tabula Muris](https://www.biorxiv.org/content/early/2017/12/20/237446)初始发布的数据为例。Tabula Muris是旨在使用标准化方法分析小鼠细胞类型的国际合作项目。其结合高通量但低覆盖率的10X数据和低通量但高覆盖率FACS分选细胞+ Smartseq2.

最初发布的数据（2017年12月20日）包含20个不同组织/器官的近100,000个细胞。

## 下载数据
和大多数scRNA-seq数据不同，Tabula Muris在[figshare](https://figshare.com/)上发布其数据，而不是上传到[GEO](https://www.ncbi.nlm.nih.gov/geo/)或[ArrayExpress](https://www.ebi.ac.uk/arrayexpress/)。使用他们文章中doi获取数据：[10.6084/m9.figshare.5715040](https://figshare.com/articles/Single-cell_RNA-seq_data_from_Smart-seq2_sequencing_of_FACS_sorted_cells/5715040) FACS/Smartseq2数据，[10.6084/m9.figshare.5715025](https://figshare.com/articles/Single-cell_RNA-seq_data_from_microfluidic_emulsion/5715025) 10X数据。可以通过手动点击doi链接下载或者通过下面的命令:

基于终端的FACS数据下载: 


```bash
wget https://ndownloader.figshare.com/files/10038307
unzip 10038307
wget https://ndownloader.figshare.com/files/10038310
mv 10038310 FACS_metadata.csv
wget https://ndownloader.figshare.com/files/10039267
mv 10039267 FACS_annotations.csv
```

基于终端的10X数据下载：:

```bash
wget https://ndownloader.figshare.com/files/10038325
unzip 10038325
wget https://ndownloader.figshare.com/files/10038328
mv 10038328 droplet_metadata.csv
wget https://ndownloader.figshare.com/files/10039264
mv 10039264 droplet_annotation.csv
```

注意，如果手动下载数据，则应在继续之前解压缩并重命名文件。

现在应该有两个文件夹：“FACS”和“Droplet”文件夹，以及各自一个注释和元数据文件。使用`head`查看文本文件的前几行（按`q`退出）：


```bash
head -n 10 data/droplet/droplet_metadata.csv
```
使用`wc`查看文件行数：

```bash
wc -l data/droplet/droplet_annotation.csv
```

**练习**
我们从FACS和10X注释了多少个细胞？

**答案**

```bash
wc -l FACS_annotations.csv
wc -l droplet_annotation.csv
#FACS : 54,838 cells
#Droplet : 42,193 cells
```
## 读取Smartseq2数据

从逗号分隔文件中读取相应的的count矩阵。然后检查结果数据框：


```r
dat = read.delim("data/FACS/Kidney-counts.csv", sep=",", header=TRUE)
dat[1:5,1:5]
```

```
##               X A14.MAA000545.3_8_M.1.1 E1.MAA000545.3_8_M.1.1
## 1 0610005C13Rik                       0                      0
## 2 0610007C21Rik                       1                      0
## 3 0610007L01Rik                       0                      0
## 4 0610007N19Rik                       0                      0
## 5 0610007P08Rik                       0                      0
##   M4.MAA000545.3_8_M.1.1 O21.MAA000545.3_8_M.1.1
## 1                      0                       0
## 2                      0                       0
## 3                      0                       0
## 4                      0                       0
## 5                      0                       0
```
数据框中的第一列是基因，将数据框rownames重命名为基因名


```r
dim(dat)
```

```
## [1] 23433   866
```

```r
rownames(dat) <- dat[,1]
dat <- dat[,-1]
```

由于这是一个Smartseq2数据集，它可能包含spike-ins，通过下述命令检查数据集：


```r
rownames(dat)[grep("^ERCC-", rownames(dat))]
```

```
##  [1] "ERCC-00002" "ERCC-00003" "ERCC-00004" "ERCC-00009" "ERCC-00012"
##  [6] "ERCC-00013" "ERCC-00014" "ERCC-00016" "ERCC-00017" "ERCC-00019"
## [11] "ERCC-00022" "ERCC-00024" "ERCC-00025" "ERCC-00028" "ERCC-00031"
## [16] "ERCC-00033" "ERCC-00034" "ERCC-00035" "ERCC-00039" "ERCC-00040"
## [21] "ERCC-00041" "ERCC-00042" "ERCC-00043" "ERCC-00044" "ERCC-00046"
## [26] "ERCC-00048" "ERCC-00051" "ERCC-00053" "ERCC-00054" "ERCC-00057"
## [31] "ERCC-00058" "ERCC-00059" "ERCC-00060" "ERCC-00061" "ERCC-00062"
## [36] "ERCC-00067" "ERCC-00069" "ERCC-00071" "ERCC-00073" "ERCC-00074"
## [41] "ERCC-00075" "ERCC-00076" "ERCC-00077" "ERCC-00078" "ERCC-00079"
## [46] "ERCC-00081" "ERCC-00083" "ERCC-00084" "ERCC-00085" "ERCC-00086"
## [51] "ERCC-00092" "ERCC-00095" "ERCC-00096" "ERCC-00097" "ERCC-00098"
## [56] "ERCC-00099" "ERCC-00104" "ERCC-00108" "ERCC-00109" "ERCC-00111"
## [61] "ERCC-00112" "ERCC-00113" "ERCC-00116" "ERCC-00117" "ERCC-00120"
## [66] "ERCC-00123" "ERCC-00126" "ERCC-00130" "ERCC-00131" "ERCC-00134"
## [71] "ERCC-00136" "ERCC-00137" "ERCC-00138" "ERCC-00142" "ERCC-00143"
## [76] "ERCC-00144" "ERCC-00145" "ERCC-00147" "ERCC-00148" "ERCC-00150"
## [81] "ERCC-00154" "ERCC-00156" "ERCC-00157" "ERCC-00158" "ERCC-00160"
## [86] "ERCC-00162" "ERCC-00163" "ERCC-00164" "ERCC-00165" "ERCC-00168"
## [91] "ERCC-00170" "ERCC-00171"
```

从列名中提取这些数据的元数据：


```r
cellIDs <- colnames(dat)
cell_info <- strsplit(cellIDs, "\\.")
Well <- lapply(cell_info, function(x){x[1]})
Well <- unlist(Well)
Plate <- unlist(lapply(cell_info, function(x){x[2]}))
Mouse <- unlist(lapply(cell_info, function(x){x[3]}))
```

查看元数据的分布信息


```r
summary(factor(Mouse))
```

```
## 3_10_M 3_11_M 3_38_F 3_39_F  3_8_M  3_9_M 
##    104    196    237    169     82     77
```

检查是否有任何技术因素混淆：


```r
table(Mouse, Plate)
```

```
##         Plate
## Mouse    B001717 B002775 MAA000545 MAA000752 MAA000801 MAA000922
##   3_10_M       0       0         0       104         0         0
##   3_11_M       0       0         0         0       196         0
##   3_38_F     237       0         0         0         0         0
##   3_39_F       0     169         0         0         0         0
##   3_8_M        0       0        82         0         0         0
##   3_9_M        0       0         0         0         0        77
```

最后，读取计算推测的细胞类型注释，并与表达矩阵中的细胞进行匹配：


```r
ann <- read.table("data/FACS/FACS_annotations.csv", sep=",", header=TRUE)
ann <- ann[match(cellIDs, ann[,1]),]
celltype <- ann[,3]
```

## 构建scater对象

要创建SingleCellExperiment对象，必须将所有细胞注释放在一个数据框中，因为实验批处理（pcr板）与供体鼠标完全混淆，我们只保留其中一个。


```r
library("SingleCellExperiment")
library("scater")
cell_anns <- data.frame(mouse = Mouse, well=Well, type=celltype)
rownames(cell_anns) <- colnames(dat)
sceset <- SingleCellExperiment(assays = list(counts = as.matrix(dat)), colData=cell_anns)
```

如果数据集包含spike-ins，在SingleCellExperiment对象中设置一个隐藏变量来跟踪它们：


```r
isSpike(sceset, "ERCC") <- grepl("ERCC-", rownames(sceset))
```

## 读取10X数据
由于10X数据的大规模和稀疏性（表达矩阵中高达90％可能是0），它通常存储为稀疏矩阵。 CellRanger的默认输出格式是.mtx文件，将稀疏矩阵存储为一列行坐标,一列列坐标，一列>0的表达值。注意，如果查看.mtx文件，发现两行标题行后紧跟一行，其详细说明完整矩阵的行数，列数和计数。由于只有坐标存储在.mtx文件中，因此每行和列的名称必须分别存储在“genes.tsv”和“barcodes.tsv”文件中。

使用“Matrix”包读取稀疏矩阵。


```r
library("Matrix")
cellbarcodes <- read.table("data/droplet/Kidney-10X_P4_5/barcodes.tsv")
genenames <- read.table("data/droplet/Kidney-10X_P4_5/genes.tsv")
molecules <- readMM("data/droplet/Kidney-10X_P4_5/matrix.mtx")
```

添加适当的行名和列名。查看read的cellbarcode，发现只有与每个细胞相关的barcode序列。考虑到10X数据每一批的cellbarcodes序列可能有重复，因此在合并数据前将批次ID与cellbarcode合并。


```r
head(cellbarcodes)
```

```
##                   V1
## 1 AAACCTGAGATGCCAG-1
## 2 AAACCTGAGTGTCCAT-1
## 3 AAACCTGCAAGGCTCC-1
## 4 AAACCTGTCCTTGCCA-1
## 5 AAACGGGAGCTGAACG-1
## 6 AAACGGGCAGGACCCT-1
```


```r
rownames(molecules) <- genenames[,1]
colnames(molecules) <- paste("10X_P4_5", cellbarcodes[,1], sep="_")
```
读入元数据和计算注释信息。


```r
meta <- read.delim("data/droplet/droplet_metadata.csv", sep=",", header = TRUE)
head(meta)
```

```
##    channel mouse.id  tissue   subtissue mouse.sex
## 1 10X_P4_0    3-M-8  Tongue        <NA>         M
## 2 10X_P4_1    3-M-9  Tongue        <NA>         M
## 3 10X_P4_2  3-M-8/9   Liver hepatocytes         M
## 4 10X_P4_3    3-M-8 Bladder        <NA>         M
## 5 10X_P4_4    3-M-9 Bladder        <NA>         M
## 6 10X_P4_5    3-M-8  Kidney        <NA>         M
```
需要用"10X_P4_5"获得这批数据的meta信息，注意到metadata表格中mouse ID和FACS数据集中mouse ID不同，这里用"-"而不是"_"作为分隔符，而且性别位于ID的中间。通过查阅文献得知，droplet和FACS技术使用相同的8只小鼠，所以修改小鼠ID，与FACS实验保持一致。


```r
meta[meta$channel == "10X_P4_5",]
```

```
##    channel mouse.id tissue subtissue mouse.sex
## 6 10X_P4_5    3-M-8 Kidney      <NA>         M
```

```r
mouseID <- "3_8_M"
```
注意：有些组织的10X数据可能来自混合样本，比如mouse id = 3-M-5/6。仍然需要格式化mouse id，但是可能与FACS数据的mouse id不一致，影响下游分析。如果小鼠不是来自近交系，那么可以使用exonic-SNP将每个细胞分配给特定小鼠，但这超出了本课程的范围。


```r
ann <- read.delim("data/droplet/droplet_annotation.csv", sep=",", header=TRUE)
head(ann)
```

```
##                        cell  tissue cell_ontology_class
## 1 10X_P4_3_AAAGTAGAGATGCCAG Bladder    mesenchymal cell
## 2 10X_P4_3_AACCGCGTCCAACCAA Bladder    mesenchymal cell
## 3 10X_P4_3_AACTCCCGTCGGGTCT Bladder    mesenchymal cell
## 4 10X_P4_3_AACTCTTAGTTGCAGG Bladder        bladder cell
## 5 10X_P4_3_AACTCTTTCATAACCG Bladder    mesenchymal cell
## 6 10X_P4_3_AAGACCTAGATCCGAG Bladder    endothelial cell
##                      cell_ontology_term_iri cell_ontology_id
## 1 http://purl.obolibrary.org/obo/CL_0008019       CL:0008019
## 2 http://purl.obolibrary.org/obo/CL_0008019       CL:0008019
## 3 http://purl.obolibrary.org/obo/CL_0008019       CL:0008019
## 4 http://purl.obolibrary.org/obo/CL_1001319       CL:1001319
## 5 http://purl.obolibrary.org/obo/CL_0008019       CL:0008019
## 6 http://purl.obolibrary.org/obo/CL_0000115       CL:0000115
```
注释中的cellID和cellbarcode格式有些差别，在匹配前进行校正。

```r
ann[,1] <- paste(ann[,1], "-1", sep="")
ann_subset <- ann[match(colnames(molecules), ann[,1]),]
celltype <- ann_subset[,3]
```

构建cell-metadata数据框

```r
cell_anns <- data.frame(mouse = rep(mouseID, times=ncol(molecules)), type=celltype)
rownames(cell_anns) <- colnames(molecules);
```

**练习** 使用组织的其它批次重复上述处理.

**答案**

```r
molecules1 <- molecules
cell_anns1 <- cell_anns

cellbarcodes <- read.table("data/droplet/Kidney-10X_P4_5/barcodes.tsv")
genenames <- read.table("data/droplet/Kidney-10X_P4_5/genes.tsv")
molecules <- Matrix::readMM("data/droplet/Kidney-10X_P4_5/matrix.mtx")
rownames(molecules) <- genenames[,1]
colnames(molecules) <- paste("10X_P4_6", cellbarcodes[,1], sep="_")
mouseID <- "3_9_M"
ann_subset <- ann[match(colnames(molecules), ann[,1]),]
celltype <- ann_subset[,3]
cell_anns <- data.frame(mouse = rep(mouseID, times=ncol(molecules)), type=celltype)
rownames(cell_anns) <- colnames(molecules)

molecules2 <- molecules
cell_anns2 <- cell_anns

cellbarcodes <- read.table("data/droplet/Kidney-10X_P7_5/barcodes.tsv")
genenames <- read.table("data/droplet/Kidney-10X_P7_5/genes.tsv")
molecules <- Matrix::readMM("data/droplet/Kidney-10X_P7_5/matrix.mtx")
rownames(molecules) <- genenames[,1]
colnames(molecules) <- paste("10X_P7_5", cellbarcodes[,1], sep="_")
mouseID <- "3_57_F"
ann_subset <- ann[match(colnames(molecules), ann[,1]),]
celltype <- ann_subset[,3]
cell_anns <- data.frame(mouse = rep(mouseID, times=ncol(molecules)), type=celltype)
rownames(cell_anns) <- colnames(molecules)

molecules3 <- molecules
cell_anns3 <- cell_anns
```

## 创建scater对象

现在已经读入多个批次的10X数据，将其合并成一个SingleCellExperiment对象。首先检查不同批次基因名字和顺序是否一致。


```r
identical(rownames(molecules1), rownames(molecules2))
```

```
## [1] TRUE
```

```r
identical(rownames(molecules1), rownames(molecules3))
```

```
## [1] TRUE
```

检查有无重复的cellIDs:

```r
sum(colnames(molecules1) %in% colnames(molecules2))
```

```
## [1] 0
```

```r
sum(colnames(molecules1) %in% colnames(molecules3))
```

```
## [1] 0
```

```r
sum(colnames(molecules2) %in% colnames(molecules3))
```

```
## [1] 0
```

检查没有问题后，将数据进行合并。
Everything is ok, so we can go ahead and combine them:


```r
all_molecules <- cbind(molecules1, molecules2, molecules3)
all_cell_anns <- as.data.frame(rbind(cell_anns1, cell_anns2, cell_anns3))
all_cell_anns$batch <- rep(c("10X_P4_5", "10X_P4_6","10X_P7_5"), times = c(nrow(cell_anns1), nrow(cell_anns2), nrow(cell_anns3)))
```

**练习**
数据集中共有多少细胞？

**答案**


创建SingleCellExperiment对象，SingleCellExperiment类的一个优势是其可以以正常矩阵或者稀疏矩阵的格式存储数据，以及HDF5格式，以有效的方式在磁盘存储和读取大型非稀疏矩阵，而不是整个加载到内存中。


```r
all_molecules <- as.matrix(all_molecules)
sceset <- SingleCellExperiment(
    assays = list(counts = as.matrix(all_molecules)),
    colData = all_cell_anns
)
```
因为这是10X数据，不包括spike-ins，可以直接存储数据。


```r
saveRDS(sceset, "kidney_droplet.rds")
```

## 进阶练习

编写R函数/脚本自动处理任何组织的任意数据类型。
