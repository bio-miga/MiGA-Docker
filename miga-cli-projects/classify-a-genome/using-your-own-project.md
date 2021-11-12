# Using Your Own Project

For this exercise it is assumed that you have already run the **Running a Genome Project** exercise. The project created in that exercise \(`pseudo`\) consisted of 6 _Psuedomonas_ genomes submitted as fasta files. A seventh _Psudomonas_ genome in the same directory had the extension `fna` so that it would not be included in the project when we added all genome files ending with `fasta`. We will now classify this seventh _Psuedomonas_ genome in relation to the genomes already in the project; _i.e._ `pseudo` will serve as our reference genome project in this exercise.

If you need to, log in and start MIGA CLI using the appropriate method for a [local installation](../starting-miga/starting-miga.md#starting-local-miga-cli) (**Starting & Stopping MiGA Docker**), MiGA [on a cluster](../starting-miga/starting-miga.md#starting-miga-singularity) (**Starting & Stopping MiGA Docker**), or [on AWS](../aws/miga\_aws\_setup.md#log-into-miga-cli-using-a-terminal) (**Setup a MiGA Instance**).

Add the dataset ending with `fna` to the MiGA project `pseudo` with the code below. The -D flag gives the name for the one genome being added. The -t flag specifies the type of genome being added to the project. The --query flag \(-q is also acceptable\) indicates that the genome being added is a query genome \(rather than a reference genome, the default\). The -i flag specifies that the dataset being uploaded is an assembled genome.

```text
cd
cd $HOME/miga-data/pseudo
miga add -P . -D P_aeruginosa -t genome --query -i assembly $HOME/miga-data/miga_genomes/pseudo/P_aeruginosa.fna
```

**Note:** It is possible to add more than one genome to a project in this way, but in that case the -D flag cannot be used. Instead, an -r flag giving a regex expression for the extraction of the genome names from the names of the files being submitted must be given. This is beyond the scope of this tutorial.  

Launch the daemon to start MiGA processing your data:  

```text
miga daemon start -P . --shutdown_when_done
```

The "shutdown-when-done" argument automatically stops the daemon when processing is complete.

After the job starts, you can display the information about the job:

```text
miga about -P .
```

Also after the job starts, you can list the datasets being processed by entering:

```text
miga ls -P . -i
```

And you can monitor the job progress by entering:  

```text
miga ls -P . -p
```

This should give something like:

```text
-         name  raw_reads  trimmed_reads  read_quality  trimmed_fasta  assembly  cds   essential_genes  ssu   mytaxa  mytaxa_scan  distances  taxonomy  stats
-         ----  ---------  -------------  ------------  -------------  --------  ---   ---------------  ---   ------  -----------  ---------  --------  -----
P_alcaligenes   -          -              -             -              done      done  done             done  done    done         done       done      done
P_fluorescens   -          -              -             -              done      done  done             done  done    done         done       done      done
  P_mendocina   -          -              -             -              done      done  done             done  done    done         done       done      done
     P_putida   -          -              -             -              done      done  done             done  done    done         done       done      done
   P_stutzeri   -          -              -             -              done      done  done             done  done    done         done       done      done
   P_syringae   -          -              -             -              done      done  done             done  done    done         done       done      done
 P_aeruginosa   -          -              -             -              done      done  done             done  done    done         queued     queued    queued
```

Because the dataset submitted was an assembled genome, assembly is the first column to have an entry. When the entry under the stats column reads "-" rather than "queued" processing is finished. 

I you started the daemon with the  `--shutdown_when_done` flag, you can also check that the project is finished by listing the files in the project daemon directory:  

```
ls daemon
```

If a file ending with `pid` is present, the daemon is still running.

If you did not include `--shutdown_when_done` when you started the daemon and the job is finished, you can stop the daemon with the command:

```
miga daemon stop -P .
```

You may view the classification results with:

```text
less data/09.distances/P_aeruginosa.intax.txt
```

which should display:

```text
Closest relative: P_alcaligenes with AAI: 67.73878174613809.

      Rank  Taxonomy  P-value            Signif.
      ----  --------  -------            -------
      root  ?         0.0                ****
    domain  ?         0.0                ****
    phylum  ?         0.0                ****
     class  ?         0.0                ****
     order  ?         0.0                ****
    family  ?         0.0                ****
     genus  ?         0.182469447956174  *
   species  ?         0.457648546144121  *
subspecies  ?         0.49936788874842   *

Significance at p-value below: *0.5, **0.1, ***0.05, ****0.01.
```

This gives _P. alcaligenes_ as the closest match among the reference genomes in `pseudo` and indicates that there is a very high probability that the genome being classified is in the same family and maybe \(p ~ 0.18\) in the same genus.
