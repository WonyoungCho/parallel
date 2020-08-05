# Slurm
Slurm is a job scheduler which is used in a cluster server. It has their own command to run program.

# SINFO
```
$ sinfo -lN
NODELIST   NODES PARTITION       STATE CPUS    S:C:T MEMORY TMP_DISK WEIGHT AVAIL_FE REASON
g-11           1      all*       mixed    6    1:6:1 120000        0      1   (null) none
g-12           1      all*       mixed    6    1:6:1 120000        0      1   (null) none
```


# SRUN
```
$ for i in {30..50};do srun -c2 --mem=5000 -J cal_${i} ldtable.py -th 0.01 -rh 101,100 -n ${i} --approx -c 2 & done
```
- `srun` : it sends a command to queue.
- `-c2` : number of cpus required per task
- `--mem=5000` : minimum amount of real memory (--mem=MB)

Note that if you do not assign `--mem`, only one task will be allocated to one node even the node has available cpus.


# SQUEUE
```
$ squeue -S LIST -l
Wed Aug  5 16:18:30 2020
             JOBID PARTITION     NAME     USER    STATE       TIME TIME_LIMI  NODES NODELIST(REASON)
              6632       all     java    user1  RUNNING      38:42 UNLIMITED      1 g-11
              6634       all     java    user1  RUNNING       6:40 UNLIMITED      1 g-12
              6635       all     java    user1  PENDING       0:00 UNLIMITED      1 (Resources)
              6636       all     java    user1  PENDING       0:00 UNLIMITED      1 (Priority)
              6637       all     java    user1  PENDING       0:00 UNLIMITED      1 (Priority)
```


# SCANCEL
```
$ scancel -uycho
```
```
$ scancel --help
Usage: scancel [OPTIONS] [job_id[_array_id][.step_id]]
  -A, --account=account           act only on jobs charging this account
  -b, --batch                     signal batch shell for specified job
  -f, --full                      signal batch shell and all steps for specified job
  -H, --hurry                     avoid burst buffer stage out
  -i, --interactive               require response from user for each job
  -M, --clusters                  clusters to issue commands to.
                                  NOTE: SlurmDBD must be up.
  -n, --name=job_name             act only on jobs with this name
  -p, --partition=partition       act only on jobs in this partition
  -Q, --quiet                     disable warnings
  -q, --qos=qos                   act only on jobs with this quality of service
  -R, --reservation=reservation   act only on jobs with this reservation
      --sibling=cluster_name      remove an active sibling job from a federated job
  -s, --signal=name | integer     signal to send to job, default is SIGKILL
  -t, --state=states              act only on jobs in this state.  Valid job
                                  states are PENDING, RUNNING and SUSPENDED
  -u, --user=user_name            act only on jobs of this user
  -V, --version                   output version information and exit
  -v, --verbose                   verbosity level
  -w, --nodelist                  act only on jobs on these nodes
      --wckey=wckey               act only on jobs with this workload
                                  charactization key

Help options:
  --help                          show this help message
  --usage                         display brief usage message
```
