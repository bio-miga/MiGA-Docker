# Installing MiGA using Singularity

These instructions are for installing MiGA in a Singularity container from a Docker image. This overcomes several disadvantages over using a Docker container in a cluster environment, the more important of which are:

* Docker containers grant superuser privileges, so it is hard to limit access to a user. Singularity runs as the user without granting superuser access.
* The Docker API client acts through the docker daemon so that resource requests made by a submission script \(*e.g.* slurm\) and actual resource used do not match. Singularity containers instead run as child processes, *i.e*. without a daemon, so slurm resource requests are honored. 

While these instructions are written for installing MiGA in a user's home directory on Michigan State University's HPCC, they should work on any cluster including Singularity.

Log into the HPCC with your username and password.

**IMPORTANT:** Installation may not work from all nodes. Initially it worked only from dev-intel18.i, but since the HPCC upgrade was completed, I have installed MiGA from dev-intel16. If you have trouble, try a different node and let John Quensen know your experience.

```text
ssh  dev-intel18
```

While in your home directory, create a singularity image of MiGA named MiGA using the command:

```text
singularity build MiGA docker://miga/miga:1.2.2.3
```

Once the image is successfully built, run the image shell to start MiGA by entering the command:

```text
singularity shell MiGA
```

MiGA must be initialized the first time the image is run using the command:

```text
miga init
```

This comammnd configures several optional features and system parameters. Most can be left at their default values (by pressng "Enter" at the end of a prompt), but some need to be manually configured. 

When asked if you want to use MyTaxa, respond "no." MyTaxa is not included in this version of MiGA becasue it would make the Docker image to large.  

When asked if you want to preserve the daemon template, responod "no." 
You will then be asked fro the type of daemon, to which you should respond "bash."  

When asked "How many jobs can I launch at once?" consider increasing the default value of 6. This should be on-half of the number of cores you intend to use, but it does not need to be large than the number of genomes you intend to process. If you set this to 10 (the maximum number of genomes in the tutorials), the tutorials will finish quicker. Since we will allow 2 CPUs per job, this will require 20 cores when running a job. This value is typically set to 32 jobs, requring 64 cores, when procesing larger "real world" jobs.  

MiGA is now installed in your home directory. Exit Singularity and close your connection to the HPCC by entering:

```
exit
exit
```

You must start a new connection before you can use MiGA.

### Optional - Installing Reference Databases

If you want to classify submitted genomes taxonomically, there are two reference databases available for the purpose. If you attach one of these to a project, then every reference genome submitted to the project will be classified.

The two reference databases available are **TypeMat\_Lite** and **Phyla\_Lite**. The December 2021 version of **TypeMat\_Lite** contains over 14,000 reference genomes from type material, and **Phyla Lite** contains reference genomes from all 41 bacterial and archaeal phyla. 

To install the latest version of **Phyla\_Lite** in `~/.miga_db`, log back in to the HPCC and while in your home directory enter:

```
singularity shell MiGA
miga get_db -n Phyla_Lite
```

Being so much larger, **TypeMat\_Lite** takes up to an hour to install. Thus, it is more conveniently installed by submitting a job. See the section **SCRIPTS FOR CLUSTER PROJECTS** for instructions.

