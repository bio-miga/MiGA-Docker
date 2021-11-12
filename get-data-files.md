# Get Data Files

Data for the exercises in this GitBook are available on GitHub at `https://github.com/jfq3/data_sets`. The bash script below downloads all of the data necessary for the exercises and puts them in a directory structure compatible with the example scripts. It also changes the extension of one of the pseudomonad genomes so that it may be set aside as a query genome for some of the exercises.

```
#! /bin/bash

# Get example data
mkdir -p $HOME/miga-data/miga_genomes
cd $HOME/miga-data/miga_genomes
mkdir a_capsulatum
mkdir dehalo
mkdir misc
mkdir pseudo
cd a_capsulatum
wget https://github.com/jfq3/data_sets/raw/master/A_capsulatum_reads.fasta.gz
gzip -d A_capsulatum_reads.fasta.gz
rm A_capsulatum_reads.fasta.gz
cd ../dehalo
wget https://github.com/jfq3/data_sets/raw/master/Dehalococcoides_genomes/dehalococcoides_genomes.tar.gz
tar xzf dehalococcoides_genomes.tar.gz
rm dehalococcoides_genomes.tar.gz
cd ../misc
wget https://github.com/jfq3/data_sets/raw/master/Miscellaneous_genomes/miscellaneous.tar.gz
tar xzf miscellaneous.tar.gz
rm miscellaneous.tar.gz
cd ../pseudo
wget https://github.com/jfq3/data_sets/raw/master/Pseudomonas_genomes/pseudomonads.tar.gz
tar xzf pseudomonads.tar.gz
mv P_aeruginosa.fasta P_aeruginosa.fna
rm pseudomonads.tar.gz
```

The resulting files and directory structure are:

```
$HOME/miga-data/miga_genomes
├── a_capsulatum
│   └── A_capsulatum_reads.fasta
├── dehalo
│   ├── GCF_000011905.1_ASM1190v1_genomic.fna
│   ├── GCF_000341655.1_ASM34165v1_genomic.fna
│   ├── GCF_000341695.1_ASM34169v1_genomic.fna
│   ├── GCF_000830925.1_ASM83092v1_genomic.fna
│   ├── GCF_001010485.1_ASM101048v1_genomic.fna
│   ├── GCF_001547795.1_ASM154779v1_genomic.fna
│   ├── GCF_001610775.1_ASM161077v1_genomic.fna
│   ├── GCF_001889305.1_ASM188930v1_genomic.fna
│   ├── GCF_002007845.1_ASM200784v1_genomic.fna
│   └── GCF_004684285.1_ASM468428v1_genomic.fna
├── misc
│   ├── Acidobacterium_capsulatum.fasta
│   ├── Acinetobacter_baumanii.fasta
│   ├── Bacillus_anthracis.fasta
│   ├── Bacillus_cereus.fasta
│   ├── Bifidobacterium_bifidum.fasta
│   ├── Campylobacter_jejuni.fasta
│   ├── Cytophaga_hutchinsonii.fasta
│   ├── Gemmatimonas_aurantiaca.fasta
│   └── Lacunisphaera_limnophila.fasta
└── pseudo
    ├── P_aeruginosa.fna
    ├── P_alcaligenes.fasta
    ├── P_fluorescens.fasta
    ├── P_mendocina.fasta
    ├── P_putida.fasta
    ├── P_stutzeri.fasta
    └── P_syringae.fasta
```
