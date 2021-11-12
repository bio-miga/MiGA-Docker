# Submitting a Genomes Project

Below is an example submission script for running the same job as in the interactive genomes project. It assumes that the genomes to be processed are in the same directory, _i.e._ `~/miga_genomes/pseudo`. The project directory is `~/miga_batch`. After starting the MiGA singularity container, the miga commands are the same as in the interactive example. The &lt;&lt; EOF ... EOF construct feeds these lines to MiGA one at a time and enables exiting the program earlier than the time requested if it finishes sooner.

```text
#!/bin/bash --login
########## SBATCH Lines for Resource Request ##########
#SBATCH --time=01:00:00         # limit of wall clock time - how long the job will run (same as -t)
#SBATCH --nodes=1               # number of different nodes - could be an exact number or a range of nodes (same as -N)
#SBATCH --ntasks=1              # number of tasks - how many tasks (nodes) that you require (same as -n)
#SBATCH --cpus-per-task=12      # number of CPUs (or cores) per task (same as -c)
#SBATCH --mem-per-cpu=2G        # memory required per allocated CPU (or core) - amount of memory (in bytes)
#SBATCH --job-name genomes      # you can give your job a name for easier identification (same as -J)

########## Command Lines to Run ##########

mkdir ~/miga_batch
cd ~/miga_batch

singularity shell ~/MiGA << EOF
miga new -P ~/miga_batch -t genomes
miga add -P . -t genome -i assembly ~/miga_genomes/pseudo/*.fasta -m run_mytaxa_scan=false,run_distances=true
time miga daemon start -t -P . --shutdown-when-done
exit
EOF
```

Times reported for this project were \(real = wall time\):

| \_ | Time |
| :--- | :--- |
| real | 19m31.236s |
| user | 76m03.670s |
| sys | 0m25.711s |
