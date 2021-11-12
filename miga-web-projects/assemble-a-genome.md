# Assemble a Genome

MiGA-Web can assemble genomes from short reads \(_e.g._ Illumina shot-gun sequences\) from an isolate, a single-cell genome, or a highly enriched metagenome. The sequences input may be raw reads in fastq format or trimmed reads in fasta format. Raw reads will be quality trimmed automatically.

For this exercise we will submit trimmed fasta reads from an isolate. Actually, this is a synthetic dataset made using Grinder to create 150 bp reads from _A. capsulatum_ with 10X coverage. The reads are unpaired.

Download the data set **A\_capsulatum\_reads.fasta.gz** from [https://github.com/jfq3/data\_sets](https://github.com/jfq3/data_sets) and decompress it \(you can do this with Win-Zip\). While MiGA-Web usually understands gzip compression, for some unknown reason this file cannot be used unless it is decompressed first.

Start Docker, MiGA-Web, and log into your MiGA-Web account. Click on **Create new project**.

Enter a name for your project in the **Path** box. You do not need to change or enter anything else on the page. Scroll to the bottom of the page and click on **Create new MiGA project**.

Upload the file as a reference dataset of type **Trimmed reads in Fasta format** and **Forward unpaired reads**. You must also enter a dataset name in the **Name** box. Remember to scroll to the bottom of the page and click on the upload button.

Turn on the daemon for the project.

If you return to the project page, the **Project Progress** bar will not indicate any progress during assembly, nor will any datasets be shown as running on the left side of the page. The only way to monitor assembly progress is to watch files appear in your results directory under the sub-directory name\_of\_your\_project/data/05.assembly/dataset\_name. Several intermediate files will appear, but when the assembly is complete there will be only three left including contig.fa. Processing and evaluation of the assembly will take a few minutes longer.

When all is finished, the progress bar on the project page will turn green. Then, clicking on **Reference datasets** will take you to another page with the name of the dataset in the upper middle of the page, with an icon next to it indicating the genome quality. Clicking on the name of the genome will take you to still another page with a menu in the middle including headings for **Ribosomal RNA \(small subunit\)**, **Quality \(essential genes\)** and **Assembly**. By clicking on these headings, you can access information on the 16S rRNA and essential genes found, a graph and statistics for the assembly, and links to download the contigs.

