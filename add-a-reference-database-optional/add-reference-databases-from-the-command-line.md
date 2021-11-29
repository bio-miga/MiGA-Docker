# Add Reference Databases from the Command Line

MiGA includes a command for downloading the databases. By default, the command places them in the hidden directory `~/.miga_db` where they are not accessible to the MiGA-Web interface. This is fine for a Singularity/cluster installation which is usable only from the command line. But in cases where the MiGA-Web interface will also be used (a local or AWS installation), the databases need to be directed to the miga-data folder.  

## For Singularity/cluster Installations

Being so much smaller, **Phyla\_Lite** takes only a few minutes to install. With **MiGA** running, the following command will install it in the default location `~/.miga_db`:

```text
miga get_db -n Phyla_Lite
```

Being much larger, **TypeMat\_Lite** can take at least an hour to install, and so should be installed by submitting a job to the cluster.   See the section **Submitting MiGA Jobs** for instructions.   

## For Local and AWS Installations

We recommend that you use the MiGA-Web interface to install reference databases in local and AWS installations, but if you want to use the command line you need to direct the databases to the miga-data directory with the -l flag. Commands for each database are:  

```
miga get_db -n Phyl_Lite -l ~/miga-data
miga get_db -n TypeMat_Lite -l ~/miga-data
```
For a local installation, you will need a fast internet connection and plenty of time to install TypeMat\_Lite. For the AWS version, Phyla\_Lite takes seconds to install and TypeMat\_Lite maybe 10 minutes. Still it is safer to install TypeMat_Lite from within a tmux session:  

```
tmux new -s install-db
miga get_db -n TypeMat_Lite -l ~/miga-data
```

Exit the tmux session with Ctrl-B d. After installation of TypeMat_Lite is finished, and assuming you have not re-entered the session, you can remove the session with:  

```
tmux kill-session -t install-db
```

The databases will not be accessible by MiGA-Web until they are linked as reference projects. To do this, in the MiGA-Webinterface, got to the Admin console and scroll to the bottom of the page. Clilck on "Link existing projects" and then "Link as reference database" for each databsed downloaded using the command line.