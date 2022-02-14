# Metagenome_assembly
WDL Workflow for metagenome assembly:
![metagenomics-pipeline drawio](https://raw.githubusercontent.com/crusher083/metagenome_assembly/master/metagenomics-pipeline.drawio.png)
Python script to generate mapping between non-redundant gene catalogue and MAGS

## Introduction to WDL workflow
### This pipeline will perform;
* Pre-processing of reads with Kneaddata
* Metagenomics assembly with Megahit
* Gene prediction
* Mapping of reads against the contigs 
* Metagenome binning using MetaBAT2 
* Quality assessment of genome bins
* Taxonomic classifications
* Gene clustering with CD-HIT-EST
* Mapping of reads to gene clusters and computing gene counts

## Requirements
 - Docker
 - JRE 9+ (tested on JRE 17)
    - `apt install openjdk-17-jre`
 - Cromwell
    - `wget https://github.com/broadinstitute/cromwell/releases/download/75/cromwell-75.jar -O cromwell.jar`
 - Python (for `input.json` generation)
   * pandas>=1.2.4

## Running the pipeline
### 1. QC and assemble  
 - Requirements
   - `wgs_files` - path to directory with WGS files and `SraRunTable.txt`
 - Output
   - uhhh
 ```sh
 # Generate inputs_py.json
 python inputs-processing/qc_and_assembly.py <wgs_files>

 # Process the data
 java \
   -Dconfig.file=cromwell-configs/kneaddata.conf \
   -jar cromwell.jar run ./wdl/1-qc_and_assemble.wdl \
   -o output-options.json \
   -i inputs_py.json
 ```

### 2. (...)

## Outputs
This pipeline will produce a number of directories and files
* assemble; contains assembled contigs
* predictgenes; gene coordinates file (GFF), protein translations and nucleotide sequences in fasta format
* metabat2; binned contigs and a summary report
* CheckM; genome assessment summary report
* gtdbtk; taxonomic classification summary file
* cluster_genes; representative sequences and list of clusters


## Mapping between gene catalogue, MAGS and eggNOG annotation
Python3 script to map non-redundant gene catalogue back to contigs, MAGS and eggNOG annotations 

## Input requirements
* clustering file - tab-delimited file with cluster ID and gene ID
* Non-redundant gene catalogue (fasta)
* Contig files in fasta
* binned contigs (MAGS) in fasta
* taxonomy files (tsv)
* EggNOG annotation file (tsv)

### Output
mapping table (tsv file) that links the non-redundant gene catalogue back to contigs, MAGs and to eggNOG annotations