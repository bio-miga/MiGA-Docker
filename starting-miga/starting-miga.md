# Starting and Stopping MiGA

## Starting Local MiGA-Web

The next time you want to use MiGA-Web, start Docker Desktop and wait until it is finished loading. Then open the terminal or **PowerShell** if you are using Windows and enter, one line at a time:


```text
docker start miga-web
docker exec -it miga-web /bin/bash
cd /home/ubuntu/miga-web/
export SECRET_KEY_BASE=`bundle exec rake secret`
export RAILS_SERVE_STATIC_FILES=true
bundle exec rails server -e production -b 0.0.0.0 -p 3000 -u Puma
```

Then start your browser and enter "localhost:9090" as the URL.

The first time you use MiGA-Web, you will be asked to create an account. You will need this account information to access your results in subsequent sessions.

To shut down MiGA-Web, return to the terminal you used above, hold down the **Ctrl key** and type "C." Open a new terminal and enter:

```text
docker stop miga-web
```

## Starting Local MiGA-CLI

To start a local installation of MiGA, start Docker Desktop and wait until it is finished loading. Then open the terminal or **PowerShell** if you are using Windows and enter:

```text
docker start miga-web
docker exec -it miga-web /bin/bash
cd /home/ubuntu/miga-data/
```
Run all of your command line programs in the `miga-data` directory.  

When you are finished, enter:

```text
exit
docker stop miga-web
```

## Starting MiGA-Singularity

In most cases you will run MiGA on a cluster by submitting jobs, but there are occasions to run it interactively. To do so, log in to the cluster and issue the following command from your home directory:

```text
singularity shell MiGA
```

To stop using MiGA, simply enter:

```text
exit
```

