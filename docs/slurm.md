# Slurm
Slurm is a job scheduler which is used in a cluster server. It has their own command to run program.

```
$ sinfo

```

# run jobs
```
$ for i in {30..50};do srun -c2 --mem=5000 -J cal_${i} ldtable.py -th 0.01 -rh 101,100 -n ${i} --approx -c 2 & done
```
- `srun` : send command to queue.
- `-c2` : 
