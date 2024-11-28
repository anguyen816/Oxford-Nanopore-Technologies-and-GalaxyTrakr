# Oxford-Nanopore-Technologies-and-GalaxyTrakr

The objective of this project is to perform ONT (Oxford Nanopore Technologies) data analysis for bacterial genome assembly and its initial characterization using GalaxyTrakr. The project aims to leverage the high-throughput sequencing capabilities of ONT to generate accurate and comprehensive bacterial genome assemblies. Using GalaxyTrakr, the project will involve uploading sequencing data, filtering for read quality and sizes, and assembling the bacterial genome from the high-quality data. Following the assembly, quality control (QC) will be conducted to ensure the accuracy and completeness of the genome. The bacterial species will be identified, and further analysis will include Multilocus Sequence Typing (MLST) to determine the genetic profile, as well as the identification of antimicrobial resistance genes, including stress and virulence genes, for relevant bacterial strains. Serotyping for Salmonella and E. coli will also be performed to identify their serological profiles. Additionally, optional annotation of the bacterial genome will be conducted to identify functional genomic elements. This workflow aims to provide detailed insights into bacterial genomics, antimicrobial resistance, and virulence factors.

# Wet Bench: 
Source: https://nanoporetech.com/document/rapid-sequencing-dna-v14-pcr-barcoding-sqk-rpb114-24

This protocol starts with extracted and quantified DNA (gram positive/ Gram negative bacteria). Expected concentration for DNA is 50-120ng/uL

Considerations: 
1. It is recomended to use the minimum of 12 barcoded samples per run (2400ng per flow cell) (max 24 barcoded samples). Underloading will result in reduced  sequencing speed and accelerated flow cell degradation. If the pores are not actively sequencing it will die off faser/ easier
2. The flow cell max use is 48 hours. You CANNOT re-use the flow cell after that. Wash and re-using flow cell are only for when you are sequencing for 2 hours or so... and even then pores might still be dying rapidly.
3. MinION/ GidIOn need to maintain 35C throughout to ensure successful sequencing.
4. Do not use flow cell thtt have less than 800 live pores



### Library prepearation (Avoid vortexing)
1.  In a PCR tube/plate. Add 18.5uL of 200ng extracted DNA (Ex: DNA concentration is 50ng/uL. Divide it by 200. Which is 4. Then 18.5 minus 4 is 14.5. So the dilution would be 18.5 of H2O and 4uL of DNA)
2.  Add 1.5uL Fragmentation Mix (Index plate) and pipette mix 10 times and spin down.
3.  Incubate at 30C for 2 mins then at 80C for 2 mins in a thermocycler and cool for 10C. Quick spin down.

### 1X bead wash
1. All the liquid should be at the bottom of the well
2. Pool all the barcoeded samples in a 1.5mL eppendorf tube
3. Add an equal amount of AMPureXP beads for the number of samples pooled and mix by pipetting
4. Spin down and pellet on a magnet for 2 mins. Remove the supernatant
5. Plate is still on the magnet. Wash the beads with 1mL 80% ethanol
6. remove the ethanol, and repeat the wash
7. Remove all residual ehtanol, and dry for 30s
8. Plate off magnet. Resuspened the pellet  in 15 EB (flick to mix)
9. Incubate for for 2 mins at RT
10. Pellet the beads on a magnet until clear
11. Transfer 12uL to a new tube/plate. This can be quantify using Qubit.

### Addition of RAP adapter
1. Add 1uL of RA+ADB mix (ex:1.5uL of RA + 3.5uL of ADB)
2. flick to mix and spin down
3. Incubate for 5 mins at RT
4. This is the final library

### Load using MinION/GridION

Source: https://community.nanoporetech.com/nanopore_learning/lessons/priming-and-loading-your-flow-cell

When the flow cell have been loaded and the runis going, Here are some few considerations:
1. Wait 20 mins to check on the run. Accurate metrics will not show until after 20 mins
2. The flow cell on the software will show green= actively sequencing. Red= blocky DNA, pore is block, Not sequecing. Yellow= adapter have bound but not read. Free adapter, and if too many = not good (Not bound)
3. Q score needs to be steady and straight line
4. Pore occupancy for RBK114.24 should be around 75-80%
5. Pore scans happens every 1.5 hours. You will see pores slowly dying off (ex: 1450 at 0 hr and decrease to 100 after 48 hours)
6. Reads passing filter: red line below blue and green line= good. above= bad

# Dry Lab/ GalaxyTrakr analysis


### 1. Create a new history/workspace
+ Log into GalaxyTrakr, "Upload Data" or Click "Share Data" & "Data Libraries."

+ If the data was shared with you, you can search for it using the search bar. Select all of the barcodes that you want to upload and "Export to History" as "Datasets" (You are uploading Fastq files).
  
### 2. Filter long reads by quality
+ Tool: Filtlong (Source: https://github.com/rrwick/Filtlong). This tool is used to remove the worst 5% of reads and reads below 1000 bp and to downsample the reads to ~100X coverage of the genome (e.g., if 6 Mb, then downsample to 600 Mb).
+ Input Fastq files using a single dataset for multiple datasets.
+ Navigate to Output thresholds and enter, for example: "600000000" for Total bases, and "95" for Keep percentage (to keep 95% of the data; however, it is common to do 90%, which means you are discarding 10% of the data), and "1000" for Min. length (keeping any reads above 1,000 bp only).
+ Theoretically, you should use the estimated size for your organism. If your coverage is 65X, it will not be affected.

  
### 3. De novo assembler for single-molecule sequencing reads
+ Tool: Flye (Sources: https://www.pnas.org/doi/full/10.1073/pnas.1604560113, https://github.com/mikolmogorov/Flye). This tool is used to select parameters for high-quality ONT reads and additional polishing iterations.
+ Input Fastq files.
+ Navigate to Mode and enter "Nano HQ (--nano-hq)" and for Number of polishing iterations, enter "4."
+ Tool's output: This will generate 4 different output files: Assembly info (general info such as how many contigs you have in total, length, etc.), Graphical fragment assembly (similar to Bandage, with a circular layout), Assembly graph, and Consensus (the actual assembly in the Fasta format).
+ This process may take time due to polishing. After all files are completed, you can use BioEdit for better visualization of the sequence alignment.
  
### 4. Quality Assessment Tool for Genome Assemblies (Can only be used when the assembly is done)
+ Tool: Quast (Source: https://academic.oup.com/bioinformatics/article/34/13/i142/5045727). This tool assesses the general quality of the assembly, such as length, GC content, etc. The difference with the Flye tool is that Flye does not provide GC content.
+ It is better to use Quast when you are running your sequence against a reference genome because you want to see if there are any duplications.
  
### 5. Identifying species using sketch
+ Tool: Sketch (Source: https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0185056). This tool is used to identify the genome species. It is a quick and dirty version of BLAST. It will also identify the closest ANI that matches your sample.
+ With this tool, you need to use the assembly from Flye.
+ Navigate to the Sketch Database and enter "Refseq." Then, for Output format, choose "Default format with per query header line; one column header line; and one reference line per hit." Additionally, for "Only report records with at least this many hits," enter "3."
+ After the task is finished, you will have the ID of your organism in the chart under "Taxname," such as "Salmonella enterica subsp. enterica serovar Heidelberg."

### 6. Scans genomes against PubMLST schemes
+ Tool: MLST (Source: https://github.com/tseemann/mlst)
+ With this tool, you need to use the assembly from Flye.

### 7. NCBI AMRFinderPlus - requires data_manager_amrfinderplus
+ Tool: amrfinderplus_db (Source: https://www.biorxiv.org/content/10.1101/550707v1)
+ With this tool, you need to use the assembly from Flye.
+ Since at this point you already know the ID of your Organism you can make your selection in the drop down bar for "Get organism-specific results"
+ Toggle on "Add the plus genes to the report". This will give you the stress genes and virulence genes

### 8. Perform serotyping of Escherichia coli Test
+ Tool: E coli Serotyper (Source: https://journals.asm.org/doi/10.1128/jcm.00008-15)
+ With this tool, you need to use the assembly from Flye.
+ Under "Are the input files FastQ or Contigs" Choose "Contigs"
+ Similarly, for Salmonella you would use the tool: SeqSero2 (Source: https://github.com/CFSAN-Biostatistics/SeqSero2-galaxy)


### 9. Prokaryotic genome annotation (Optional)
+ Tool: Prokka (Source: https://academic.oup.com/bioinformatics/article/30/14/2068/2390517)
+ With this tool, you need to use the assembly from Flye.
+ Under "Analysis mode", choose "allele mode" and for "Algorith for BWA mapping", choose "mem"
+ Navigate to "Locus tag prefix", enter in " N" then under "Force GenBank/ENA/DDJB compliance" choose yes. Genus and Species name fields are optional. Lastly, under "Additional output" only select "Standard GenBank file. If the input was a multi-FASTA, then this will be a multi-GenBank, with one record for each sequence (.gbk)"

## Galaxytrakr ONT Workflow
Essentially, you can analyze your sample(s) by combining all the steps listed above using the workflow. (this workflow doesn not contain "Prokka")
+ Navigate to "Shared Data", choose "Workflow" search for "Nanopore Datasets-QC_assembly_profiling_salmonella". Click the drop down button and select "import"
+ Toggle yes for " Send results to a new history" and youcan give a "history name"











