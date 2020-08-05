Fabric is job spliter, which is based on SSH protocol in Python.

<http://www.fabfile.org/>

It is useful when you have several computers which are linked in same gateway, not clustered or no scheduler.

# Basic
Basic use is simple.
```
from fabric import Connection
ip='192.168.0.10'
cmd='hostname'
result = Connection(ip).run(cmd, hide=True)
msg = "Ran {0.command!r} on {0.connection.host}, got stdout:\n{0.stdout}"
print(msg.format(result))
```
```
$ python3 
Ran 'hostname' on 192.168.0.10, got stdout:
g-master
```

# Multi-tasking
```
from fabric import Connection
import parmap
import multiprocessing as mp
import pandas as pd

def kill(ccmd):
    cmd='pkill adi'
    result = Connection('192.168.10.1'+ccmd[0]).run(cmd, hide=True)
    out='192.168.10.1'+ccmd[0]+': killed.'

    return out

def uptime(ccmd):
    cmd='uptime'
    result = Connection('192.168.10.1'+ccmd).run(cmd, hide=True)
    out= result.connection.host+'\t'+result.stdout[-6:-1]

    return out

def memory(ccmd):
    cmd='free -h'
    result = Connection('192.168.10.1'+ccmd).run(cmd, hide=True)
    out= result.stdout

    return out

def par_main():
    #a=parmap.map(kill,iplist, pm_pbar=False, pm_processes=mp.cpu_count())
    a=parmap.map(uptime,iplist, pm_pbar=False, pm_processes=mp.cpu_count())
    b=parmap.map(memory,iplist, pm_pbar=False, pm_processes=mp.cpu_count())

    tb=pd.DataFrame(b)[0].str.split('\s+|\n',expand=True)

    c=tb.values.tolist()
    d=[]
    col=['IP','CPU','MEM','USE','FREE','BUF','AVB']
    for i,j in enumerate(r0):
        d.append(a[i]+'\t'+c[i][8]+'\t'+c[i][9]+'\t'+c[i][10]+'\t'+c[i][12]+'\t'+c[i][13])

    fdf=pd.DataFrame(d)[0].str.split('\s+',expand=True)
    fdf.columns=col
    print(fdf)

if __name__=='__main__':
    r0=list(range(1,10))
    r=r0+r0

    df=pd.read_csv('jobname.lst',header=None)

    flist=df[0].tolist()
    print('Total number of jobs:',len(flist))

    clist=[['%02d'%r[i],flist[i]] for i in range(len(flist))]
    iplist=['%02d'%i for i in r0]

    par_main()
```
```
Total number of jobs: 10
                IP    CPU  MEM   USE  FREE   BUF  AVB
0   192.168.10.101   0.05  62G  484M   61G  503M  61G
1   192.168.10.102   0.05  62G  490M   61G  495M  61G
2   192.168.10.103   0.05  62G  485M   61G  490M  61G
3   192.168.10.104   0.05  62G  482M   61G  493M  61G
4   192.168.10.105   0.05  62G  483M   61G  514M  61G
...
```
