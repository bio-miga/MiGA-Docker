# submitting-large-jobs

## Resource Request

In the submitted job examples, `cpus-per-task` was set to 12. To run jobs with more genomes in a timely fashion, the number of CPUs/cores used needs to be increased. So, the first step is to decide on how many cores you want to use. It should be an even number and as large as practical but does not need to be more than 2 times the number of genomes.

MiGA does not need much memory per CPU, but sometimes run-time errors occur with large jobs. If this happens, you can restart the job from the point it failed. But to decrease the chance of it happening, increase the `memory per CPU`, to 4G for example, but make sure `mem-per-cpu` times `cpus-per-task` does not exceed the total available memory.

If the job is killed for exceeding the wall time requested, it can be restarted from the point it ended. But to give some idea about the time necessary, with `cpus-per-task` set to 64, processing 160 genomes in a clade project took approximately 16 hours.

## Daemon Configuration

The number of jobs \(processes\) MiGA can run at one time is defined in the daemon configuration file `.miga_daemon.json`, located in your home directory. The default file looks like this:

```text
{
  "created": "2020-10-25 12:09:10 -0400",
  "updated": "2020-10-25 12:09:10 -0400",
  "type": "bash",
  "latency": 2,
  "maxjobs": 6,
  "ppn": 2,
  "cmd": "{{vars}} {{miga}} run -r '{{script}}' -l '{{log}}' -e",
  "var": "{{key}}={{value}}",
  "varsep": " ",
  "alive": "ps -p '{{pid}}' | tail -n +2 | wc -l",
  "kill": "kill -9 '{{pid}}'",
  "format_version": 1
}
```

In this configuration the maximum number of jobs MiGA can run simultaneously is 6, and `ppn` is 2. 6 times 2 is 12, the number of cores requested in the example submission scripts.

So that the daemon configuration does not limit processing time, you need to edit the value for `maxjobs` to one-half of the `cpus-per-core` in your submission script.

## Restarting Jobs

If a submitted MiGA job is interrupted, it can usually be restarted from the point it left off. To do so, simply comment out lines having to do with creating the project and adding genomes and re-submit. For example, for a clade project, lines would be revised to look like:

```text
singularity shell ~/MiGA << EOF
# miga new -P ~/miga_clade -t clade
# miga add -P . -t genome -i assembly ~/dehalo_genomes/*.fna -m run_mytaxa_scan=false,run_distances=true
time miga daemon start -t -P . --shutdown-when-done
exit
EOF
```

