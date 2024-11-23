# Oxford-Nanopore-Technologies-and-GalaxyTrakr

The objective of this project is to perform ONT (Oxford Nanopore Technologies) data analysis for bacterial genome assembly and its initial characterization using GalaxyTrakr. The project aims to leverage the high-throughput sequencing capabilities of ONT to generate accurate and comprehensive bacterial genome assemblies. Using GalaxyTrakr, the project will involve uploading sequencing data, filtering for read quality and sizes, and assembling the bacterial genome from the high-quality data. Following the assembly, quality control (QC) will be conducted to ensure the accuracy and completeness of the genome. The bacterial species will be identified, and further analysis will include Multilocus Sequence Typing (MLST) to determine the genetic profile, as well as the identification of antimicrobial resistance genes, including stress and virulence genes, for relevant bacterial strains. Serotyping for Salmonella and E. coli will also be performed to identify their serological profiles. Additionally, optional annotation of the bacterial genome will be conducted to identify functional genomic elements. This workflow aims to provide detailed insights into bacterial genomics, antimicrobial resistance, and virulence factors.

# Wet Bench: (Coming Soon)





# Dry Lab/ GalaxyTrakr analysis


### 1. Create a new history/ workspace
+ Log into GalaxyTrakr, "Upload Data" or Click "Share Data" & "Data Libraries"

+ If the data was shared with you you can search for it using the search bar. Select all of the baracodes that you want to upload and "Export to History" as a "Datasets" (You are uploading Fast.q files)

### 2. Filter long reads by quality. 
+ Tool: Filtlong (Source: https://github.com/rrwick/Filtlong) This Tool is to Remove the worst 5% reads and reads below 1000bp  and to downsample the reads to ~100X coverage of the genome. (Ex: if 6 Mb then down sample to 600Mb)
+ Input Fastq files using single dataset for multiple datasets
+ Navigate to Output thresholds and enter in, for example:  "600000000" for Total bases an "95" for Keep percentage (to keep 95 percentage of the data, but it is commone to do 90 percent also, which means you are throwing away 10 percentage of data) and "1000" for Min. length (keeping any reads that is above 1,000 only).
+ Theoretically you should use the estimate size for your organism. And if your cover is 65X, it will not be affected

### 3. De novo assembler for single molecule sequencing reads 
+ Tool: Flye (Sources: https://www.pnas.org/doi/full/10.1073/pnas.1604560113, https://github.com/mikolmogorov/Flye) This tools is to select parameters for high qulaity ONT reads and additional polishing ierations.
+ Input Fastq files
+ Navigate to Mode and enter "Nano HQ (--nano-hq) and for Number of polishing iterations enter "4"
+ Tools output: This will generate 4 different output files: Assembly info (General info such as how many contigs you have total, length..etc), Graphical fragment assembly (similar to bandage, with circle layout), Assembly graph, Consensus (The actualy assembly in the Fasta format)
+ This process may take long due to polishing. After all files are completed, you can use Bioedit to have a better visualization of the sequence alignment

### 4. Quality Assessment Tool for Genome Assemblies( Can only be use when the assembly is done)
+ Tool: Quast (Source: https://academic.oup.com/bioinformatics/article/34/13/i142/5045727) This tool assess the of the general quality of the assembly such as the length, GC content..etc. the different with the Flye tool is that Flye does not tell you the GC content.
+ It is better to use Quast when you are running your sequence agaisnt against  a reference because you want to se if therea are any duplications.










