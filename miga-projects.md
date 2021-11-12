# MiGA Projects

MiGA can run 4 types of projects using 3 types of data submitted in any of 3 formats. It is important to understand the terms applied to all of these types, so to clarify:

### Types of data:

* **Genome**: A genome assembled from sequencing an isolate
* **Scgenome**: A genome assembled from sequencing a single cell \(single-cell amplified genome or **SAG**\)
* **Popgenome**: A genome \(or bin\) assembled from sequencing a sample \(*e.g.*, soil, water, skin\) containing multiple species \(metagenome-assembled genome or **MAG**\)

Input data need not be finished genomes; they may also be contigs, scaffolds, or bins. One of the functions of MiGA is to evaluate a genome \(*i.e.*. the input data\) for completeness and quality.

Input genomes are referred to as datasets; they may be input as either reference or query datasets.

### Types of projects:

* **Genomes**: A collection of genomes - from Genome or Scgenome data types
* **Clade**: A collection of closely related genomes, e.g. of the same species - from Genome or Scgenome data types
* **Metagenomes**: A collection of popgenomes and/or viromes - from Popgenome data types
* **Mixed**: A mixed collection of genomes, popgenomes, and viromes - from any of the data types above

All projects are started by submitting reference datasets. Query datasets may be submitted to Genomes, Metagenomes, and Mixed projects to be classified relative to the reference datasets they contain.

Clade projects differ in that all genomes are compared against each other by ANI and AAI. Query genomes are not submitted to clade projects.

### Types of input:

* **Assembly in fasta format** - can be complete, contigs, scaffolds, or bins
* **Raw reads in fastq format** - un-trimmed and un-filtered short reads to be assembled
* **Trimmed reads in fasta format** - trimmed and filtered short reads to be assembled

Reads to be assembled should come from pure cultures, single cells, or at least highly enriched cultures. Assembling reads from complex environments is likely to fail.



