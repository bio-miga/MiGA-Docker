# Add a Query Sequence to a Genomes Project

Add *P. aereuginosa* as a query genome to the previous *Pseudomonas* genomes project. It will be classified relative to the reference genomes in the project.

```text
#!/bin/bash --login
########## SBATCH Lines for Resource Request ##########
#SBATCH --time=02:00:00         # limit of wall clock time - how long the job will run (same as -t)
#SBATCH --nodes=1               # number of different nodes - could be an exact number or a range of nodes (same as -N)
#SBATCH --ntasks=1              # number of tasks - how many tasks (nodes) that you require (same as -n)
#SBATCH --cpus-per-task=12      # number of CPUs (or cores) per task (same as -c)
#SBATCH --mem-per-cpu=2G        # memory required per allocated CPU (or core) - amount of memory (in bytes)
#SBATCH --job-name pseudo      # you can give your job a name for easier identification (same as -J)

########## Command Lines to Run ##########

mkdir ~/miga_batch
cd ~/miga_batch

singularity shell ~/MiGA << EOF
miga add -P . -D P_aeruginosa -t genome --query -i assembly ~/miga_genomes/pseudo/P_aeruginosa.fna
time miga daemon start -t -P . --shutdown-when-done
exit
EOF
```

After the job finishes, entering the following:

```text
less ~/miga_batch/data/09.distances/P_aeruginosa.intax.txt
```

returns:

```text
Closest relative: P_alcaligenes with AAI: 67.73878174613809.

      Rank  Taxonomy  P-value             Signif.
      ----  --------  -------             -------
      root  ?         0.0                 ****
    domain  ?         0.0137151106833494  ***
    phylum  ?         0.0137151106833494  ***
     class  ?         0.0137151106833494  ***
     order  ?         0.0137151106833494  ***
    family  ?         0.0955245428296439  **
     genus  ?         0.202358036573628   *
   species  ?         0.53055822906641
subspecies  ?         0.702598652550529
   dataset  ?         0.705726660250241

Significance at p-value below: *0.5, **0.1, ***0.05, ****0.01.
```

The interpretation is that the query genome _P. aeruginosa_'s closeset match among the reference genomes already in the project is _P. alcalegenes_; the two belong to the same order and possibly to the same genus.

Times reported for this project were \(real = wall time\):

| \_ | Time |
| :--- | :--- |
| real | 1m11.631s |
| user | 1m05.713s |
| sys | 0m02.269s |

