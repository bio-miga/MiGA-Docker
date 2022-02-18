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

When this command is run to initialize MiGA, all dependencies will be configurated automatically except for several optional features and system parameters that need to be manually configured, such as MyTaxa. MyTaxa is not included in this version because it would make the image size too large. So please type “no” when the system asks if you want to use MyTaxa. Also, please type “bash” when the system asks how to submit jobs. For all other questions, accept the defaults by entering `Enter`.

MiGA is now installed in your home directory. Close your connection to the HPCC. You must start a new connection before you can use MiGA.

### Optional - Installing Reference Databases

If you want to classify submitted genomes taxonomically, there are two reference databases available for the purpose. If you attach one of these to a project, then every reference genome submitted to the project will be classified.

The two reference databases available are **TypeMat\_Lite** and **Phyla\_Lite**. The December 2020 version of **TypeMat\_Lite** contains over 13,500 reference genomes from type material, and **Phyla Lite** contains reference genomes from all 40 bacterial and archaeal phyla. The following command will install the latest version of **Phyla\_Lite** in `~/.miga_db`:

```text
miga get_db -n Phyla_Lite
```

Being so much larger, **TypeMat\_Lite** takes approximately an hour to install. Thus, it is more conveniently installed by submitting a job. See the section **Submitting MiGA Jobs** for instructions.

