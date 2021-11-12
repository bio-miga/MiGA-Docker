# Exploring Results

The tutorials include how to retrieve some of the more important results for each case: comparing relationships among genomes, finding clades, and evaluating genomes for completeness and contamination. To retrieve more detailed information, it is helpful to understand MiGA's directory structure. The following sub-directories are included for each project under the project directory:

```text
.
├── daemon
│   └── daemon.json
├── data
│   ├── 01.raw_reads
│   ├── 02.trimmed_reads
│   ├── 03.read_quality
│   ├── 04.trimmed_fasta
│   ├── 05.assembly
│   ├── 06.cds
│   ├── 07.annotation
│   │   ├── 01.function
│   │   │   ├── 01.essential
│   │   │   └── 02.ssu
│   │   ├── 02.taxonomy
│   │   │   └── 01.mytaxa
│   │   └── 03.qa
│   │       ├── 01.checkm
│   │       └── 02.mytaxa_scan
│   ├── 08.mapping
│   │   ├── 01.read-ctg
│   │   └── 02.read-gene
│   ├── 09.distances
│   │   ├── 01.haai
│   │   ├── 02.aai
│   │   ├── 03.ani
│   │   ├── 04.ssu
│   │   └── 05.taxonomy
│   ├── 10.clades
│   │   ├── 01.find
│   │   ├── 02.ani
│   │   ├── 03.ogs
│   │   ├── 04.phylogeny
│   │   │   ├── 01.essential
│   │   │   └── 02.core
│   │   └── 05.metadata
│   └── 90.stats
├── metadata
└── miga.project.json
```

The sub-directories under data and beginning with 01 through 10 correspond to MiGA's sequential steps in processing data beginning with raw paired fastq files loaded into 01.raw\_reads. Not all steps need to be done. In most of the tutorials, we submitted assembled genomes in fasta format; they were loaded into 05.assembly and processing began there. In the genome assembly exercise, we began by loading already trimmed and quality filtered data into 04.trimmed. The genome projects ended with step 09.distances. Step 10.clades was performed only for the clade project. Each of these sub-directories beginning with 01 through 10 contain results for the associated data processing step.

Some of the results are in text format \(sometimes compressed\), *e.g.*, tables, sequence files, gff files, nwk files. Others are in pdf format or Rdata format. The information in each file is explained in the [MiGA workflow section](https://manual.microbial-genomes.org/part5/workflow) of the MiGA manual. If you are using MiGA on a cluster, you can download the files individually using FileZilla or a similar program.

### A Taxonomy Summary Script

For projects including classification, Fang Yuan has written a script to summarize the results. For each classified genome it determines the closest reference genome, the distances to it, and writes the results to the file summary.csv. To use this script, move into the project/data/09.distance/05.taxonomy directory and run the following commands:

```text
wget https://github.com/jfq3/Miscellaneous-scripts/raw/master/miga_sumdb.sh
chmod 750 miga_sumdb.sh
./miga_sumdb.sh
```

For the miscellaneous genomes project in these tutorials, this produces:

| name | closest | haai | aai | ani |
| :--- | :--- | :--- | :--- | :--- |
| Acidobacterium\_capsulatum | Silvibacterium\_bohemicum\_GCA\_001006305 | 99.9508952380952 | 60.0831465538606 |  |
| Acinetobacter\_baumanii | Pseudomonas\_pharmafabricae\_GCA\_002835605 | 98.7412788461538 | 48.5834215277602 |  |
| Bacillus\_anthracis | Bacillus\_flexus\_NBRC\_15715\_GCA\_001591565 | 98.9199428571429 | 59.5616830477613 |  |
| Bacillus\_cereus | Bacillus\_flexus\_NBRC\_15715\_GCA\_001591565 | 98.9702788461539 | 59.9991400823551 |  |
| Bifidobacterium\_bifidum | Bifidobacterium\_scardovii\_JCM\_12489\_\_\_DSM\_13734\_GCA\_000770985 | 99.7717980769231 | 68.9492871800872 |  |
| Campylobacter\_jejuni | Campylobacter\_helveticus\_GCA\_900176295 | 99.1678666666667 | 66.2667775679713 |  |
| Cytophaga\_hutchinsonii | Nafulsella\_turpanensis\_ZLM\_10\_GCA\_000346615 | 99.9811470588235 | 50.1443577165562 |  |
| Gemmatimonas\_aurantiaca | Gemmatimonas\_phototrophica\_NZ\_CP011454 | 99.9633431372549 | 68.505494616812 |  |
| Lacunisphaera\_limnophila | Oleiharenicola\_lentus\_GCA\_004118375 | 99.9875247524753 | 68.0251176025193 |  |

### A genome completeness and quality script

John Quensen has written a python script to summarize the completeness, contamination, and quality of each genome in a project and write it to a tab-delimited file that may be viewed as is or loaded into a spread-sheet program like Excel. It takes two arguments: the path to the MiGA project and the name of the output file. Download and run the script by entering the following commands:

```text
wget https://github.com/jfq3/Miscellaneous-scripts/raw/master/miga_completeness.py
chmod u+x 
python ./miga-completeness.py /path/to/miga/project completness_summary.txt
```

For the miscellaneous genomes project in these tutorials, this produces:


```
Completeness    Contamination   Quality Genome
94.6            0.9             90.1    P_putida
94.6            0.9             90.1    P_stutzeri
95.5            0.9             91.0    P_alcaligenes
94.6            0.9             90.1    P_syringae
93.7            0.9             89.2    P_fluorescens
94.6            0.9             90.1    P_mendocina
```

### Browse the results with MiGA-Web

MiGA was originally provided as a web-based program, and the results were delivered via a web browser. You can view MiGA results generated on a cluster in the same manner if you first install the Docker version of MiGA on your computer. Steps are as follow:

1. If you have not already done so, install the [Docker Version](https://www.docker.com/products/docker-desktop) appropriate to your system \(Windows, Mac OS or Linux\).
2. If you have not already done so, install MiGA-Web:
   * Open a terminal and enter the following, one line at a time:

     ```text
     docker pull miga/miga:1.0.2.0
     docker run -p 9090:3000 -it -v C:/miga:/root/miga-data -v db_volume:/miga/db --name miga miga/miga:1.0.2.0 /bin/bash
     cd miga/
     export SECRET_KEY_BASE=`bundle exec rake secret`
     bundle exec rails server -e production -b 0.0.0.0 -p 3000 Puma
     ```

     The docker run line creates the MiGA project directory as C:/miga on your computer. You may use a different directory name if you wish.

   * Enter control C and close the terminal.
   * Open a new terminal and enter:

     ```text
     docker stop miga
     ```

   * Close the terminal.
3. Compress the results on the cluster:
   * Using tar:

     ```text
     cd project_directory
     tar czf project_name.tar.gz .
     ```

   * Using MiGA archive. With MiGA running on the cluster:

     ```text
     cd project_directory
     miga archive -o project_name.tar.gz -P .
     ```  

     The MiGA archive option has the advantage that unnecessary files are not included in the archive. Thus the archive file is smaller and the MiGA results take up less drive space on your computer.
4. Download the compressed file to the project directory on your computer and decompress it. The installation commands above created the project directory as C:/miga-web.

   ```text
   tar xzf project_name.tar.gz
   ```

5. Start MiGA on your computer.
   * Start Docker desktop.
   * Open a terminal and enter, one line at a time:

     ```text
     docker start miga
     docker exec -it miga /bin/bash
     cd miga/
     export SECRET_KEY_BASE=`bundle exec rake secret`
     bundle exec rails server -e production -b 0.0.0.0 -p 3000 Puma
     ```

   * Leave the terminal running until you are finished browsing the results \(see below\).
   * Open a web browser, enter "localhost:9090" as the URL and log into MiGA.
6. Link the project to MiGA.
   * Go to the Admin console page.
   * Click on "Link existing projects"
   * Under the name of the project, click either "Link publicly" or "Link privately." The choice only affects where the project will be listed.
7. Open the project and browse the results. Get explanations for each item; the "Learn more" links take you directly to the appropriate section of the MiGA manual. The download links copy the item to your downloads directory.
8. After you have finished, end MiGA as above after you installed it:
   * Enter control C into the terminal and close it.
   * Open a new terminal and enter:

     ```text
     docker stop miga
     ```

   * Close the terminal.

