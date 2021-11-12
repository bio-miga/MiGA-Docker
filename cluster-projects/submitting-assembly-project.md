# Submitting and Assembly Project

Assemble a genome from a trimmed fasta file produced by shotgun sequenceing an isolate.

```text
#!/bin/bash --login

########## SBATCH Lines for Resource Request ##########
#SBATCH --time=4:00:00           # limit of wall time - how long the job will run
#SBATCH --nodes=1                # number of different nodes - exact number or range
#SBATCH --ntasks=6               # number of tasks - how many tasks (nodes) you require
#SBATCH --cpus-per-task=2       # number of CPUs (cores) per task
#SBATCH --mem-per-cpu=2G         # memory required per allocated CPU (or core)
#SBATCH --job-name assemble      # job name for easier identification

########## Command Lines to Run ##########

mkdir ~/test_assemble
cd  ~/test_assemble

singularity shell ~/MiGA << EOF
miga new -P ~/test_assemble -t genomes
miga add -P ~/test_assemble -t genome -i trimmed_reads_single ~/miga_genomes/a_capsulatum/A_capsulatum_reads.fasta
time miga daemon start -t -P . --shutdown-when-done
exit
EOF
```

A report on the assembled genome can be obtained with:

```text
less  ~/test_assemble/data/07.annotation/01.function/01.essential/A_capsulatum_reads.ess/log
```

which reveals:

```text
! Collection: dupont_2012
! Essential genes found: 105/111.
! Completeness: 94.6%.
! Contamination: 2.7%.
! Multiple copies:
!   2 tRNA-synt_1d: tRNA synthetases class I (R).
!   2 GrpE: GrpE.
!   2 TIGR00436: era: GTP-binding protein Era.
! Missing genes:
!   TIGR00389: glyS_dimeric: glycine--tRNA ligase.
!   TIGR00408: proS_fam_I: proline--tRNA ligase.
!   TIGR00471: pheT_arch: phenylalanine--tRNA ligase, beta subunit.
!   TIGR00775: NhaD: Na+/H+ antiporter, NhaD family.
!   TIGR00810: secG: preprotein translocase, SecG subunit.
!   TIGR02387: rpoC1_cyan: DNA-directed RNA polymerase, gamma subunit.
```

The assembled genome can be retrieved as `~/test_assemble/data/05.assembly/A_capsulatum_reads.AllContigs.fna`

Times reported for this project were \(real = wall time\):

| \_ | Time |
| :--- | :--- |
| real | 8m42.971s |
| user | 8m26.007s |
| sys | 0m05.480s |

