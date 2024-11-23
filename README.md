# Oxford-Nanopore-Technologies-and-GalaxyTrakr

The objective of this project is to perform ONT (Oxford Nanopore Technologies) data analysis for bacterial genome assembly and its initial characterization using GalaxyTrakr. The project aims to leverage the high-throughput sequencing capabilities of ONT to generate accurate and comprehensive bacterial genome assemblies. Using GalaxyTrakr, the project will involve uploading sequencing data, filtering for read quality and sizes, and assembling the bacterial genome from the high-quality data. Following the assembly, quality control (QC) will be conducted to ensure the accuracy and completeness of the genome. The bacterial species will be identified, and further analysis will include Multilocus Sequence Typing (MLST) to determine the genetic profile, as well as the identification of antimicrobial resistance genes, including stress and virulence genes, for relevant bacterial strains. Serotyping for Salmonella and E. coli will also be performed to identify their serological profiles. Additionally, optional annotation of the bacterial genome will be conducted to identify functional genomic elements. This workflow aims to provide detailed insights into bacterial genomics, antimicrobial resistance, and virulence factors.

# Wet Bench Intructions: (Coming Soon)





# Dry Lab Intructions:


### 1. Create a new history/ workspace
+ Log into GalaxyTrakr, "Upload Data" or Click "Share Data" & "Data Libraries"

+ If the data was shared with you you can search for it using the search bar. Select all of the baracodes that you want to upload and "Export to History" as a "Datasets" (You are uploading Fast.q files)

### 2. Filter long reads by quality. 
+ Tool: Filtlong (Source: https://github.com/rrwick/Filtlong) This Tool is to Remove the worst 5% reads and reads below 1000bp  and to downsample the reads to ~100X coverage of the genome. (Ex: if 6 Mb then down sample to 600Mb)
+ Input Fastq files using single dataset for multiple datasets
+ Navigate to "Output thresholds" and enter in, for example:  "600000000" for Total bases an "95" for Keep percentage (to keep 95 percentage of the data, but it is commone to do 90 percent also, which means you are throwing away 10 percentage of data) and "1000" for Min. length (keeping any reads that is above 1,000 only).
+ Theoretically you should use the estimate size for your organism. And if your cover is 65X, it will not be affected










