# installing-typemat-lite

This script is provided as a convenient way of obtaining the latest version of the TypeMat\_Lite database which contains more than 13,500 genomes from type material. By attaching it to a genome project, reference genomes submitted to the project are classified relative to the closest match in the database.

By default the database is installed in the directory `~/.miga_db`. It may be directed to a different directory with the -l flag. The resource request section is for a SLURM job scheduler.

```text
#!/bin/bash --login
########## SBATCH Lines for Resource Request ##########
#SBATCH --time=02:00:00         # limit of wall clock time - how long the job will run (same as -t)
#SBATCH --nodes=1               # number of different nodes - could be an exact number or a range of nodes (same as -N)
#SBATCH --ntasks=1              # number of tasks - how many tasks (nodes) that you require (same as -n)
#SBATCH --cpus-per-task=2       # number of CPUs (or cores) per task (same as -c)
#SBATCH --mem-per-cpu=16G       # memory required per allocated CPU (or core) - amount of memory (in bytes)
#SBATCH --job-name get_db       # you can give your job a name for easier identification (same as -J)

########## Command Lines to Run ##########
## Download the latest version of TypeMat_Lite to ~/.miga_db.

cd
singularity shell ~/MiGA << EOF
miga get_db
exit
EOF
```

