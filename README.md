Pipeline designed for Vibrio genus
## Installation ##
Dowload the pipeline using the command:
	`git clone https://github.com/lfdelzam/primer_desing.git`

The pipeline uses the programs:

[snakemake](https://snakemake.github.io) 3.13.3

[muscle](http://www.drive5.com/muscle) v3.8.1551

[Degeprime](https://github.com/EnvGen/DEGEPRIME) v1.1.0

[mfeprimer](https://github.com/quwubin/MFEprimer-3.0/releases/tag/v3.2.3) 3.2.3

## Create conda environment ##

conda create -n primers -c bioconda snakemake=3.13.3 muscle=3.8.1551

Degeprime and MFEprimer must be manually downloaded if you want to use a different version. 

## Usage ##

set pipeline parameters in config_primer_design.json using the command:
`nano config_primer_design.json`
and modify the parameters and save changes by taping `ctrl x` and tape `y`:


	  "work_dir": "/absolute/path/to/primer_design/",

	  "tar_gff_files": "/absolute/pat/to/genome_gff_files.tar",   -- tar file downloaded from NCBI, "no required if "gene_sequences" provided --

	  "tar_file_genomes": "/absolute/path/to/genome_fasta.tar",   -- tar file downloaded from NCBI, "no required if "gene_sequences" provided --

	  "NCBI_gene_name": "your option", -- NCBI name of the target gene (required), e.g., "GroEL", "'16S ribosomal RNA'"  --

	  "NCBI_gene_region": "your option", -- standard NCBI region name of the target gene (required), e.g., "CDS", "exon"  --

	  "gff_file": "/absolute/path/to/gff_with_selected_gene_annotation.gff", -- if file doesn't exits, it will be generated using "tar_gff_files" --

	  "gene_sequences": "/absolute/path/to/gene_sequeneces.fna",  -- if not provided it will be generated using "tar_gff_files", "tar_file_genomes", and "gff_file" --

	  "muscle_params": "-maxiters 3",

	  "trim_degeprime": "-min 0.9",

	  "degeneracies": "4,12",

	  "primer_sizes": "17,18,19,20,21,22",

	  "primer_selection_parameters": "-c 0.99 -g 40", -- -c stands for coverage (fraction) and -g for minimum GC content (%) --

	  "amplicon_size": "50,550" -- minimum and maximum amplicon size --
   
   	  "threads": 6, -- Number of CPUs to be used --
      
      	  "Output_suffix_name": "your option", -- Remember to rename it if you changed one of the parameters --

run the pipeline using the commands:

	`conda activate primers`
	`snakemake -s design_primer --cores <number of threads>`

## output ##

Directory `Results`:

		file "List_<Output_suffix_name>": Lists all possible primer pairs combinations of selected primers

		file "primers_<Output_suffix_name>_table": Lists of selected primers. It includes primer name, sequences, reverse complement, degeneracy, length, GC range, TM range


## intermediare output ##

Directory `MSA`:

	It contains the multiple sequence alignment generated by muscle file `MSA_<Output_suffix_name>`, and the file `timmed_align_file_from_MSA_<Output_suffix_name>` generated by Degeprime

	Directory `Potential_primers`:

	It contains the files generated by MFEprimer (dimer and hairpin primer evaluation) and by Degeprime
