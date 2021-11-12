# Submit a Clade Project

A clade project is run using closely related genomes, *e.g.*, genomes of the same species. For this example we use 10 *Dehalococoides* genomes downloaded from NCBI.    

```text
#!/bin/bash --login
########## SBATCH Lines for Resource Request ##########
#SBATCH --time=02:00:00         # limit of wall clock time - how long the job will run (same as -t)
#SBATCH --nodes=1               # number of different nodes - could be an exact number or a range of nodes (same as -N)
#SBATCH --ntasks=1              # number of tasks - how many tasks (nodes) that you require (same as -n)
#SBATCH --cpus-per-task=12      # number of CPUs (or cores) per task (same as -c)
#SBATCH --mem-per-cpu=2G        # memory required per allocated CPU (or core) - amount of memory (in bytes)
#SBATCH --job-name MiGA_clade  # you can give your job a name for easier identification (same as -J)

########## Command Lines to Run ##########

rm -rf ~/miga_clade
mkdir ~/miga_clade
cd ~/miga_clade

singularity shell ~/MiGA << EOF
miga new -P ~/miga_clade -t clade
miga add -P . -t genome -i assembly ~/dehalo_genomes/*.fna -m run_mytaxa_scan=false,run_distances=true
time miga daemon start -t -P . --shutdown-when-done
exit
EOF
```

Entering

```text
less  ~/miga_clade/data/10.clades/02.ani/miga-project.class.tsv
```

presents a table with the genomes in the first column and their clade assignment in the second column:

```text
GCF_000011905_1_ASM1190v1_genomic       1
GCF_000341655_1_ASM34165v1_genomic      2
GCF_000341695_1_ASM34169v1_genomic      2
GCF_000830925_1_ASM83092v1_genomic      3
GCF_001010485_1_ASM101048v1_genomic     3
GCF_001547795_1_ASM154779v1_genomic     2
GCF_001610775_1_ASM161077v1_genomic     2
GCF_001889305_1_ASM188930v1_genomic     3
GCF_002007845_1_ASM200784v1_genomic     2
GCF_004684285_1_ASM468428v1_genomic     2
```

A tree of the genomes in Newick format can be obtained from `~/miga_clade/data/10.clades/02.ani/miga-project.class.nwk`

Times reported for this project were \(real = wall time\):

| \_ | Time |
| :--- | :--- |
| real | 20m45.196s |
| user | 95m26.944s |
| sys | 1m03.999s |

