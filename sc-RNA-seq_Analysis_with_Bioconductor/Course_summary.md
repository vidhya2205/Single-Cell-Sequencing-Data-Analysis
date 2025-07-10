# Single Cell RNA Sequencing: Course Summary

---

## 1. Introduction

### 1.1. Comparison of sc-RNA seq and Bulk RNA-seq

Bulk RNA-seq provides the average expression of genes across all cells in a tissue that was sent for sequencing. This does not account to heterogeneity in the tissue and provide information as to which cells in the tissue express the gene. To overcome this the concept of single cell sequencing was introduced. This sequencing data can provide the distribution of expression levels across a population of cells. Based on the protocols used it can range from 100 to 1 million cells per study.

### 1.2. How is the sample prepared for the protocol?

-	Tissue dissection via chemical or enzymatic methods to obtain a suspension of cells. Sometimes instead of single cell isolation, we can use single nuclei especially when dissociation of tissues is difficult. However, nuclei contains unprocessed RNA’s with intact introns which also need to be accounted for.
-	Filter out cells that we would like to study further (optional step) based in membrane markers (FACS)
-	Capture the single cells into individual reaction containers. This can be done using 3 methods. FACS (Fluorescent assisted cell sorting), Droplets, or Nano wells.
-	Extract RNA from each well
-	Reverse transcription of RNA to cDNA
-	Amplification of cDNA (in vitro transcription/PCR)
-	Sequencing
-	Processing of raw data (generate the count matrix with rows as genes and columns as cells). This involves demultiplexing to group information based on cell barcodes (to obtain cell specific reads) and UMI(Unique molecular identification to group reads from one transcript to account of PCR duplicates). This is followed by normalization and feature selection.
-	Downstream analysis includes reduced dimensional space, clustering & marker identification, trajectory analysis and spatial tissue reconstruction.

### 1.3. Currently used methods for sc-sequencing

The methods can be classified either based on type of cell capture/isolation and transcript quantification. 

**Based on the on type of cell capture/isolation** 

-	Microtitre plate based – Uses FACS/ laser deflection plates to isolate cells into individual wells (pipetting/ microdissection). Taking a picture of the cell before library preparation can help identify damages cell or wells that have doublets (2 or more cells) and remove them. Further, automated FACS can provide additional information regarding cell size and well coordinates. However, the drawback is it is low through put.
-	Microfluidic array based – Uses an integrated system to trap cells in a plate. Only 10% of the cells will be captured on to the plate with nanowells. The size of cells is significant as the nanowells in the plate are of 1 particular size. Additionally it is a bit expensive.
-	Microfluidic droplet based- It encapsulates each cell within a droplet along with a bead that adds the cell barcode. Pooled sequencing of the cells is possible in this method. However, has low coverage and lesser number of transcripts detected.
**Based on transcript quantification **
Transcript quantification is the process of counting how many copies of each gene are active in each individual cell.
Full length transcript quantification involves a uniform gene coverage across the whole transcript, similar to bulk RNA-sequencing.  This is restricted to plate cell isolation based sequencing methods. It is useful for studying splice variants. 
Tag based transcript quantification is more commonly used and sequences either the 3’ or 5’ ends of the transcript. They have unique molecular identifiers (UMI) attached to them to add a tag regarding the transcript from which the reads originate. It contains 2 barcodes, one to indicate the cell from which the read originates (cell barcode) and other from which transcript the read was amplified from (UMI). It does not provide information regarding the isoforms and unambiguous alignment of the read. 

The type of protocol is chosen based in the cost of sequencing per cell, no. of cells and biological question to be answered.

**Based on the biological question-**

-	If we want to characterize the composition of a heterogenous population of cells(tissue), we need to sequence more number of cells and haven an unbiased capture of cells.
-	To characterize a specific subpopulation of cells in a tissue, we have to use techniques to isolate those cells first, this can be done via FACS. Hence we can use techniques that are compatible with lower cells and higher sequencing depth.
-	If we want to study isoforms we have to go for full length sequencing however the transcript quantification will be low. 
-	To study a rare population of cells, which have no known markers, more cells have to be sequenced and hence cost would also be high.
Main point to be noted is that low throughput methods have higher sensitivity 
### 1.4.  Challenges in data analysis
-	There are no biological replicates. Cells are grouped based on similarity. And comparisons between the groups of cell is done.
-	Due to the low starting material (less number of cells of one type), the data is sparse. Additionally many genes may go undetected due to cell cell variation. This cell to cell variation can be due to technical issues uneven PCR amplification. These can be solved by increasing transcript capture and reducing amplification bias. Data normalization is also quite significant to avoid misinterpretation of the data.
-	Another commonly observed challenge in sc-seq data is batch effects arising from technical variations such as differences in sample preparation, sequencing platforms, reagents, or even processing times.

### 1.5. Key assumptions and how to correct them

-	Transcript counting (read counting)- There is an assumption in bulk-RNA seq that the number of cDNA sequenced is proportional to the amt of RNA expressed in a cell.  This can be avoided by UMI tags (Unique molecular identifiers) that indicates the transcript from which the read originates.
-	The length of the gene also affects the count (expression of the gene) this is accounted to by normalization techniques such as TPM (transcript per million).

### 1.6. Read alignment and Quantification of droplet based sc-RNA-seq

Each sc-RNA seq read has 3 key piece of information –

1.	cDNA fragments that identifies the RNA transcript to which it maps in the genome
2.	cell barcode (CB) indicating the cell from which this read (RNA) was expressed.
3.	UMI (Unique molecular identifier) to indicate the transcript from the the read originates via PCR amplification (this collapses PCR duplicate issue)

Paired end sequencing is used, where read 1 contains the CB and UMI while read 2 contains the transcript sequence.

**Overall Workflow**

-	Mapping cDNA fragments to reference genome
-	Assigning reads to genes
-	Assigning reads to the cell with the help of CB (demultiplexing)
-	Counting the number of unique RNA molecules in each cell with the help of UMI (deduplication)

### 1.7. SingleCellExperiment (SCE)

The fundamental data structure used in the sc- analysis in R is the SingleCellExperiment class. It stores multiple information pertaining the single cell experiment we are analysing in a synchronized manner, some of which include–

1.	Matrix of counts (raw data) – Its is feature (gene) by sample (cell) matrix of expression quantification. Each value in the matrix represents the expression quantification of the corresponding gene in the corresponding cell. 
2.	Additional data relating to genes such as gene length, genome location, type of gene etc.. (rowData)
3.	Information pertaining to the cells such as tissue origin, patient donor, processing batch, disease status, treatment exposure etc.. (colData)
4.	From the raw data other normalized data can be produced and stored such as normalized counts (log normalized, TPM normalized, dimensionally reduced data etc..).

The demonstration for creation, manipulation of SCE objects in R can be found in [this notebook](Intro_SingleCellExpreiment_class.Rmd) and [html file](https://vidhya2205.github.io/Single-Cell-Sequencing-Data-Analysis/sc-RNA-seq_Analysis_with_Bioconductor/Intro_SingleCellExpreiment_class.html).


