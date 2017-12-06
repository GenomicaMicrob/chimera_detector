# chimera_detector
A simple script to detect chimeric sequences in 16S/18S metagenomic samples.

This script is part of a metagenomic [pipeline](https://github.com/GenomicaMicrob/metagenomic_pipeline) we use at our lab to analyse metagenomic fingerprinting Illumina sequences.

NGS 16S or 18S metagenomic sequences sometimes have chimeric sequences produced during the PCR amplification of the samples. These sequences have to be identified and remove from the samples in order to have a more faithfull representation of the diversity present.

Two basic approaches are taken to identify these chimeric sequences, a database-dependent aproach and an idenpendent one. Chimera_detector takes the first approach since good databases are available in our [metagenomic pipeline](https://github.com/GenomicaMicrob/metagenomic_pipeline). The core process is done by [vsearch](https://github.com/torognes/vsearch), it takes as input one or several fasta files, compares their sequences to a database of your election, and produces a new fasta file free of chimeras.

## Dependencies
- [vsearch](https://github.com/torognes/vsearch), please make sure to install it first.
- a database(s)

This shell script (bash) was tested in linux Ubuntu.

## Installation
1. Download the latest release to any directory
2. Make the script executable: `chmod +x chimera_detector.vX.X.X` (vX.X.X corresponds to the number of the latest release)
3. Download the databases from [figshare](https://figshare.com/account/projects/20254/articles/4829176)
4. Uncompress them: `tar xzf mgclassifier_dbs.tar.gz`
5. Create an appropiate directory to house the script and databases: `sudo mkdir /opt/mg_pipeline/scripts /opt/mg_pipeline/databases` for this you'll have to be a super user with `sudo`. You can use a different directory, but the script points to this one, you'll have also to correct the script to point to the other directory.
6. Move everything to their appropiate directories: `sudo mv chimera_detector... /opt/mg_pipeline/scripts && sudo mv *.fasta /opt/mg_pipeline/databases`
7. For easier running of the script from any folder, make a symbolic link: `sudo ln -s /opt/mg_pipeline/scripts/chimera_detector.vX.X.X /usr/bin/chimera_detector`

## Usage
From the directory where your files reside, type `chimera_detector *.fasta`

For multiple files in the directory type:  `chimera_detector *.fna` (any extension is valid)

For a single file type the name of the file:  `chimera_detector file.fasta`

For more than one file, but not all, type the names of the files:  `chimera_detector file1.fna file2.fna`

Sorry, it cannot get files from subdirectories (e.g. /*.fasta )

## Output
The script writes the resulting files free of chimeras in a new subdirectory, called chimera_detector.year-month-day_hr:min, in fasta format. Also three files:
- `chimera_detetor.csv` with the number of sequences, chimeras, and chimera-free seqs.
- `chimera_detetor.log` a log of the analyses done, mainly from vsearch.
- `chimera_detetor.report` a report of the script; this file is used by mg_reporter for compiling a comprehensive report of the whole pipeline.

## Results
22,267 16S V4 sequences (mean length 228.7 bases, 5.18 million bases) in 4 files were analysed in a 64 core 128 GB RAM Dell PowerEdge R810 server with a SSD with the following results:

| sample | chimera-free |	chimera |
|---|---|---|
| F3D0 |	7,111 |	617 |
| F3D150 |	4,840 |	633 |
| F3D9 |	6,566 |	455 |
| Mock |	4,640 |	98 |

Total time of analysis: 6 minutes 27 seconds
