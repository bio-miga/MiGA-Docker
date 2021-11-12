# Add Reference Databases from the Command Line

Installation of the databases using the command line interface is the same for both local and cluster installations.

Being so much smaller, **Phyla\_Lite** takes only a few minutes to install. With **MiGA** running, the following command will install it in the default location `~/.miga_db`:

```text
miga get_db -n Phyla_Lite
```

Being much larger, **TypeMat\_Lite** takes at least an hour to install, even with a fast internet connection. For a local installation of MiGA, install **TypeMat\_Lite** in the same directory with the following command:

```text
miga get_db -n TypeMat_Lite
```

For a cluster installation of MiGA, it is better to install **TypeMat\_Lite** by submitting a job. See the section **Submitting MiGA Jobs** for instructions.

