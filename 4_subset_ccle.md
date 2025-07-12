# Motivating Question

Every analysis that a computational biologist undertakes should be motivated by a clearly stated biological question. Otherwise, how will you know if the data you have is appropriate and what analysis should be done?

So let's do a bit of role-playing. Imagine that we are a team of biologists working for a biotech developing drugs to treat cancer. The biotech is called Figure One Lab (What else did you think I was going to call it?). Our mission is to make drugs that interfere with skin cancer metastasis.

To see if our drugs work, we need to be able to test any anti-metastatic drugs we make on cells that model metastatic skin cancer cells. To start, we have decided to use cancer cell lines, because they are commercially available and relatively easy to grow at the bench. Given the dozens of skin cancer cell lines we can choose from, it's not clear which cell line we should use. We just know that we want to use a cell line that resembles metastatic skin cancer cells.

We start with a bit of literature review. We come across this paper: Riker, A.I., Enkemann, S.A., Fodstad, O. et al. The gene expression profiles of primary and metastatic melanoma yields a transition point of tumor progression and metastasis. *BMC Med Genomics* **1**, 13 (2008). DOI: [10.1186/1755-8794-1-13](https://doi.org/10.1186/1755-8794-1-13). Licensed under CC BY 2.0.

From the paper:

> We have utilized gene microarray analysis and a variety of molecular techniques to compare 40 metastatic melanoma (MM) samples...to 42 primary cutaneous cancers...
>
> ...the metastatic melanomas express higher levels of genes such as MAGE, GPR19, BCL2A1, MMP14, SOX5, BUB1, RGS20, and more.

The authors describe transcriptional differences between metastatic melanomas and primary melanomas. This seems promising. This motivates us to ask: **Based on the gene expression profiles of the metastatic melanomas studied by Riker et al., which skin cancer cell line should we use?**

Fortunately, we can access the RNA sequencing (RNA-seq) profiles of 1000+ cell lines from the Cancer Cell Line Encyclopedia. We can do some simple exploration of this data to recommend a skin cancer cell line appropriate for testing our drugs.

We depend on you, the sole computational biologist on the team, to explore that data and make a speedy recommendation for which cell line we should use.

## Cancer Cell Line Encyclopedia

Now we are going to apply what you have learned so far to clean and explore RNA-seq data from the Cancer Cell Line Encyclopedia (CCLE).

The CCLE is a comprehensive and publicly accessible data resource that provides detailed genetic and pharmacological profiles of many human cancer cell lines. The creation of this resource began in 2008 as a collaboration between the Broad Institute and the Novartis Institutes for BioMedical Research. They aimed to catalogue and characterize a comprehensive collection of human cancer cell lines and to make standardized, high-quality molecular data from these cell lines available for researchers everywhere. Before the establishment of the CCLE, cancer researchers faced reproducibility challenges due to the lack of standardized cancer cell line datasets, making it difficult to compare results or draw broad conclusions about cancer biology and treatment responses. The CCLE addressed these challenges.

### 🔬 Types of Data Available

1. **Cell Line Annotations**
   - Tissue of origin, cancer subtype, culture conditions, etc.

2. **Genomic Data**
   - Mutation data (WES-based)
   - Copy number alterations (CNA)
   - Fusion calls
   - Microsatellite instability (MSI) status

3. **Transcriptomic Data**
   - RNA-Seq gene expression (TPM, raw counts)
   - Isoform-level expression

4. **Proteomic Data**
   - Mass-spectrometry based protein expression levels (for some cell lines)

5. **Epigenomic Data**
   - DNA methylation (Illumina 450k array)

6. **Pharmacological Data**
   - Drug sensitivity assays (IC50, AUC, EC50) from PRISM, GDSC, and CTRP.

### 📂 File Formats

Most files are available as:

- `.csv` or `.tsv` (text-based, tabular)
- `.gct` (Gene Cluster Text format, for gene expression matrices)
- `.maf` (Mutation Annotation Format)
- `.rds` (R data object – when accessing through R packages)
- `.h5` or `.hdf5` (for larger matrix data, e.g., DepMap data) (DepMap short for Dependency Map)
- `.xlsx` (less frequently)

### 🌐 How to Access CCLE Data

✅ **1. Web Portal**

- Main site: [https://portals.broadinstitute.org/ccle](https://portals.broadinstitute.org/ccle)
- Newer data is often found via DepMap Portal: [https://depmap.org/portal](https://depmap.org/portal)

✅ **2. Direct Downloads**

- From DepMap Downloads page: [https://depmap.org/portal/download/](https://depmap.org/portal/download/)

✅ **3. R/Bioconductor Packages**

- `depmap` R package (simplifies access)

```R
library(depmap)
data <- depmap_copyNumber()
```

- Other useful packages: `TCGAbiolinks`, `ExperimentHub`

✅ **4. Google Cloud / Terra**

- Raw and processed CCLE data available via [https://terra.bio](https://terra.bio)
- For advanced users needing scalable computing (e.g., Jupyter/Workbench)

### 📊 Common Use Cases

- Identify cancer-type specific expression signatures
- Discover drug response biomarkers
- Study mutations and their functional impacts
- Integrate CCLE data with TCGA for in vitro/in vivo comparisons

## RNA-Seq

Since RNA-seq data is one of the major data assets in the CCLE and the data type we will explore in this and future courses, it behooves us to define what it is.

In brief, RNA-seq is a next-generation sequencing technique used to study gene expression. It quantifies the transcriptome — the complete set of RNA transcripts — of a given biological sample, measuring the number of RNA transcripts corresponding to each gene. RNA-seq used in well-designed experiments can capture how the transcriptomes of biological samples change across different conditions, such as "Control" vs "Treated", or "Primary Tumor" vs "Metastasis". It is a powerful tool for understanding complex biology and is ubiquitous in every area of biology.

## Download CCLE RNA-Seq Data

First things first. Where do you even download CCLE RNA-seq data? Often in computational biology work, it's not so obvious which sites and which links allow you to download the right data files. More likely than not, you will have to download several files that seem promising, read them into R or Python, and inspect each one with your eyes in order to figure out which files are the ones you actually want.

For this section, I will spare you that trouble. I have already done that work for you. The files we need are already in `cloud/project/data`. However, I want you to still retrace the steps to get those files:

1. Go to [https://depmap.org/portal/data_page/?tab=allData](https://depmap.org/portal/data_page/?tab=allData)
2. Select a file set to view: **CCLE 2019**
3. Download **"CCLE_RNAseq_genes_counts_20180929.gct.gz"**. This records the number of each transcript measured, or counted, in each RNA-seq sample. We call this the counts matrix or the counts data.
4. Download **"Cell_lines_annotations_20181226.txt"**. This is the metadata corresponding to the counts data.
5. Move both downloaded files to `cloud/project/data`.

The downloaded data was released with this publication: Ghandi et al. Next-generation characterization of the Cancer Cell Line Encyclopedia. *Nature* **569**, 503–508 (2019). DOI: [10.1038/s41586-019-1186-3](https://doi.org/10.1038/s41586-019-1186-3).

I hope that you can appreciate how unclear it often is to navigate to the data we need. This is a common issue for computational biologists.

## Need for More Compute

Because many modern biological datasets, including this one, are quite large, analyzing them often requires significant computational resources.

To run the code below, I had to upgrade my Posit Cloud plan from **Cloud Free ($0/month)** to **Cloud Basic ($25/month)**.

I do not expect you to upgrade to Cloud Basic for this course. I want to keep this course free.

However, this means that you will not be able to work with the full CCLE RNA-seq dataset with just Cloud Free. As a workaround, I am going to output a subset of the full CCLE RNA-seq dataset for you to work on in `"4_ccle_explore.qmd"`. This won't change the essential skills you will still learn; it just makes the size of the dataset manageable in Cloud Free.

## Treat ChatGPT Like Your Private Tutor

I provide the code I used to output a subset of the CCLE RNA-seq dataset below. I do not expect you to run through the code blocks below because it requires compute that is not available in Cloud Free. I do, however, expect you to read through the following code blocks, look at the output under each code block, and annotate what each code block does. I have essentially turned my code into a code-annotation exercise for you to get more experience parsing code.

One of the main reasons why I created this exercise is that it reflects what computational biologists do on a daily basis. We frequently have to go through other people's code, which usually has functions that we have not seen before, and must figure it out on the spot.

While reading my code, you may run into functions we have not explicitly covered before. But because you already understand the basics of how R works, I am confident that you can reason your way through anything I throw at you. And given that ChatGPT and other tools like it are such powerful learning companions, I expect that they will be very useful to you in parsing code.

Let's get you some experience using ChatGPT to parse code. For the following questions, provide a written response with the help of ChatGPT.

### What does the `biomaRt` package do?

```R
# Your written response here
# The biomaRt package in R provides a powerful interface to query BioMart databases, particularly Ensembl, to access a wide variety of biological data. It is part of the Bioconductor project and is especially useful in genomics and bioinformatics workflows.
```

### What does `table()` do?

```R
# Your written response here
# The table() function in R is used to create frequency tables—it counts how many times each unique value (or combination of values) appears in a vector or a set of vectors.
```

### What does the following line of code do?

```R
ccle_counts <- read.csv(paste0(getwd(),"/data/CCLE_RNAseq_genes_counts_20180929.gct.gz"), header = TRUE, sep = "\t", skip = 2)
```

```R
# Your written response here

Reads a tsv (tab separated value) and saves it in ccle_counts variable.
```

## Subset CCLE RNA-seq Data

Again, I do not expect you to run through the code blocks below because it requires compute that is not available in Cloud Free. I do, however, expect you to read through the following code blocks, look at the output under each code block, and annotate what each code block does.

Treat ChatGPT like your private tutor here. The goal here is for you to learn how to use ChatGPT to decipher code you are seeing for the first time. I want you to appreciate just how powerful a learning companion ChatGPT is. By understanding the code I present here, you are equipped to write more advanced code needed in the next notebook.

```R
{r cache=TRUE}
# Your annotation here
library(tidyverse)
library(biomaRt)
```

```R
{r cache=TRUE}
# Your annotation here
# I faced repeated crashes when trying to load this data on posit cloud.
# I installed R studio RStudio 2025.05.1+513 "Mariposa Orchid" Release (ab7c1bc795c7dcff8f26215b832a3649a19fc16c, 2025-06-01) for windows
#
# I installed the latest R at the time (R 4.5.1) but ran with compatibiliy issue with biomaRt not being compatible with the latest R version.
#
# It's a remainder for me to try to avoid latest version because of the compatibility issue. if needed use the latest version cautiously.
ccle_counts <- read.csv(paste0(getwd(),"/data/CCLE_RNAseq_genes_counts_20180929.gct.gz"), header = TRUE, sep = "\t", skip = 2)
# print(ccle_counts) # Running this crashed PositCloud temporarily
# head(ccle_counts,10) # Wanted to see the column structure of the table
dim(ccle_counts) #dim() shows the dimension of the data table
# Could also be done using str() and glimpse()
# glimpse(ccle_counts)
# str(ccle_counts)

# Due to repeated crashes of Posit Cloud I had to run the following codes in R Studio on my local pc.
```

```R
{r cache=TRUE}
# Your annotation here
# Although the file extension is text, it is basically a tsv (tab separated file). It's being read by the read.csv function
ccle_meta <- read.csv(paste0(getwd(),"/data/Cell_lines_annotations_20181226.txt"), header = TRUE, sep = "\t")
print(ccle_meta)
```

```R
{r cache=TRUE}
# Your annotation here
# Selecting a specific column using select() function
ccle_meta %>% dplyr::select(Site_Primary) %>% table()
```

```R
{r cache=TRUE}
# Your annotation here
# Counting the number of skin, kidney and CNS hits
logical_vector_a <- ccle_meta$Site_Primary %in% c("skin", "kidney", "central_nervous_system")
sum(logical_vector_a)
# print(logical_vector_a)
```

> *Notice how I have included kidney and brain cancer cell lines as well. It's always nice to have something to compare skin cancer cell lines to.*

```R
{r cache=TRUE}
# Your annotation here
# Using the logical_vector_a to get the CCLE ID, and then seleting 20 ids using head()
ccle_meta$CCLE_ID[logical_vector_a] %>% head(20)
```

```R
{r cache=TRUE}
# Your annotation here
ccle_counts %>% colnames() %>% head(20)
# having a look at the column names
```

```R
{r cache=TRUE}
# Your annotation here
# match the CCLE_ID values from 'ccle_meta', but only those rows where 'logical_vector_a' is TRUE.
logical_vector_b <- colnames(ccle_counts) %in% ccle_meta$CCLE_ID[logical_vector_a]
# Subset 'ccle_counts' to retain only the columns corresponding to the TRUE values in 'logical_vector_b'
ccle_counts_subset <- ccle_counts[,logical_vector_b]
# Combine the 'Name' and 'Description' columns from the original 'ccle_counts' with the selected expression columns.
ccle_counts_subset <- cbind(ccle_counts[,c("Name", "Description")], ccle_counts_subset)
print(ccle_counts_subset)
```

```R
{r cache=TRUE}
# Your annotation here
# Connect to the Ensembl database using the "useast" mirror.
# We're specifying the "hsapiens_gene_ensembl" dataset, which contains human gene annotations.
ensembl <- useEnsembl("ensembl", dataset = "hsapiens_gene_ensembl", mirror = "useast")
# Query the Ensembl database using getBM (get BioMart).
# We're asking for a list of genes where:
# - biotype is 'protein_coding' (i.e., protein-coding genes only)
# - We want three pieces of information (attributes):
#     1. 'ensembl_gene_id' — the Ensembl gene ID (e.g., ENSG00000141510)
#     2. 'hgnc_symbol' — the gene symbol from HGNC (e.g., TP53)
#     3. 'gene_biotype' — to confirm it's 'protein_coding'
coding_genes_hgnc <- getBM(attributes = c('ensembl_gene_id', 'hgnc_symbol', 'gene_biotype'),
                           filters = 'biotype',
                           values = 'protein_coding',
                           mart = ensembl)
print(coding_genes_hgnc)
```

```R
{r cache=TRUE}
# Your annotation here
# Fltering ccle_counts_subset to keep only protein-coding genes (by HGNC symbols).
ccle_counts_subset <- ccle_counts_subset[ccle_counts_subset$Description %in% coding_genes_hgnc$hgnc_symbol, ]
# Reassign the row numbers so that the resulting data frame has a clean default index (1, 2, 3, ...).
# This is useful if previous subsetting left irregular row names.
ccle_counts_subset <- data.frame(ccle_counts_subset, row.names = NULL)
print(ccle_counts_subset)
```

```R
{r cache=TRUE}
# Your annotation here
# writing the data to a file
write.csv(ccle_counts_subset, paste0(getwd(),"/outs/ccle_subset_rnaseq_counts.csv"), row.names = FALSE)
```

## Next

Now that we have a smaller subset of the CCLE RNA-seq dataset to work with in Cloud Free, you are ready to go on to `"4_explore_ccle.qmd"`, where you will resume active coding again.
```
