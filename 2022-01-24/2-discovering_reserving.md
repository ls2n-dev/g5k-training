# Discovering, visualizing and reserving Grid'5000 resources

At this point, you should now be connected to a site frontend, as indicated by your shell prompt (`<login>@f<site>: ~ %`). This machine will **ONLY** be used to reserve and manipulate resources on this site, using the *OAR software suite* will be the only way to request resources before using them. 

## Hardwares
*https://www.grid5000.fr/w/Hardware*
### Summary
```bash
     8 sites
    38 clusters
    760 nodes
    15694 CPU cores
    314 GPUs
    101.49 TiB RAM + 6.0 TiB PMEM
    515 SSDs and 950 HDDs on nodes (total: 1.42 PB)
    616.4 TFLOPS (excluding GPUs)
```
### By Family
- [Clusters](https://www.grid5000.fr/w/Hardware#Clusters)
- [Processors](https://www.grid5000.fr/w/Hardware#Processors)
- [Memory](https://www.grid5000.fr/w/Hardware#Memory)
- [Networking](https://www.grid5000.fr/w/Hardware#Networking)
- [Storage](https://www.grid5000.fr/w/Hardware#Storage)
- [Accelerators (GPU, Xeon Phi)](https://www.grid5000.fr/w/Hardware#Accelerators)
- [Nodes models](https://www.grid5000.fr/w/Hardware#Nodes_models)

## Discovering and visualizing
There are several ways to learn about the site's resources and their status:


### On SSH connection
The site's MOTD (message of the day) lists all clusters and their features. 

```bash
Welcome to Grid'5000

** Useful links:
 - users home: https://www.grid5000.fr/w/Users_Home
 - usage policy: https://www.grid5000.fr/w/Grid5000:UsagePolicy
 - account management (password change): https://api.grid5000.fr/ui/account
 - support: https://www.grid5000.fr/w/Support

** Connect to a site:
 - to connect to any of the Grid'5000 sites:
     grenoble lille luxembourg lyon nancy nantes rennes sophia
   (from here) $ ssh <site>
 - mind configuring the ssh aliases (proxycommand) on your workstation to
   connect directly to the Grid'5000 sites from the outside:
   (from outside) $ ssh <site>.g5k
 - or use the Grid'5000 VPN: https://www.grid5000.fr/w/VPN

** Data on access.grid5000.fr (aka access-{north,south}.grid5000.fr):
 - home directories on access machines are not synchronized, access machines
   should not be used to store data.
 - to copy data from/to a site, use the site NFS mount point linked in the home:
   (from outside) $ scp files login@access.grid5000.fr:<site>/
 - or copy directly from/to the site using the ssh alias (proxycommand):
   (from outside) $ scp files login@<site>.g5k:

See https://www.grid5000.fr/w/Getting_Started for more information.
```

Additionally, it gives also the list of current or future downtimes due to maintenance, which is also available from at https://www.grid5000.fr/status/ .

```
** Warning: 1 event in progress
--> #Incident at #Nantes from 2021-09-03@16:30 : air conditioning failure, ecotype nodes unavailable
    https://intranet.grid5000.fr/bugzilla/show_bug.cgi?id=13386
```

### On the wiki
Site pages on the wiki (e.g. [Nantes:Home](https://www.grid5000.fr/w/Nantes:Home)) contain a detailed description of the site's hardware and network.

### On specific status pages
The page information links to the resource status on each site, with two different visualizations available:
- the current placement and queued jobs status displayed by Monika (see [Nantes's current status](https://intranet.grid5000.fr/oar/Nantes/monika.cgi)) **in LIVE**
- the current and planned resources reservations in a Gantt Diagram History (see [Nantes's current status](https://intranet.grid5000.fr/oar/Nantes/drawgantt-svg/)) 

## Allocating and accessing resources with OAR

OAR is the **resources and jobs management system** (a.k.a batch manager) used in Grid'5000, just like in traditional HPC centers (commonly with SLURM, PBS, etc.). However, settings and rules of OAR that are configured in Grid'5000 slightly differ from traditional batch manager setups in HPC centers, in order to match the requirements for an experimentation testbed and **not for production use**! 

In Grid'5000 the **smallest unit of resource managed by OAR is the core (cpu core)**, but by default a OAR job reserves a host (physical computer including all its cpus and cores, and possibly gpus). Hence, what OAR calls nodes are hosts (physical machines). In the `oarsub` resource request (`-l` arguments), `node` is an alias for `host`, so both are equivalent. But prefer here using `host` for consistency with other arguments and other tools that expose host not node. 

[OAR Documentation here](http://oar.imag.fr/documentation)

### Some useful (control) commands

#### `oarsub` : submit jobs
- use option `-t besteffort` for *BestEffort" jobs which are low priority jobs: they can be killed by the scheduler to favour higher priority jobs. *BestEffort* jobs do not consume your quota.
- use option `--project <group name>` when your account is associated to (check with `groups` on CLI or on the [user management portal](https://api.grid5000.fr/stable/users))

#### `oarstat`: check job status
- `oarstat -u [-f]`:  show your present and future jobs. Optionaly add more information
- `oarstat -j <jobid> [-f]`: show the status of a specific job. Optionaly add more information 
- `oarstat -j 123456 --json`: same but with a json output. Can be piped to `jq`: ` oarstat -j 123456 --json | jq '.[].command'`

#### `oardel`: delete a job
- `oardel JOBID`

### Passive and interactive modes
#### Interactive
In interactive mode (use option `-I`), a shell is opened on the first default resource (i.e. node) of the job (or on the frontend, if the job is of type `deploy`). In interactive mode, we have 3 ways to kill a job (free a resource) :
- when the job's `shell` is closed (logout of your session)
- when the job's `walltime` is reached. 
- when you explicitly perform `oardel` with the job ID.

To reserve a job/resource in an interactive mode (connected to job's node shell), just do: 
- here oar will allocates by default a single host for one hour (by default)
  ```bash
  oarsub -I
  ```
  As soon as the resource becomes available, you will be directly connected to the reserved resource with an interactive shell, as indicated by the shell prompt, and you can run commands on the node: `lscpu` or on the web with [Gantt diagram / Monika](https://www.grid5000.fr/w/Status#Resources_reservations_.28OAR.29_status).
  
**Before the walltime ends, if you logout from your active session in this mode your reservation will be ended immediately**

### Reserving node/core resources
#### Part of a node (cores)
- To reserve only 1 CPU core in interactive mode, run:
  ```bash
  oarsub -l core=1 -I -t besteffort
  ```  
  - check with `nproc` (divide number by 2 for physical cores) 
  
- When reserving only a share of the node's cores, you will have a share of the memory with the same ratio as the cores. If you take the whole node, you will have all the memory of the node. If you take half the cores, you will have half the memory, and so on... You cannot reserve a memory size explicitly.
- When reserving several CPU cores, there is no guarantee that they will be allocated on a single node. To ensure this, you need to specify that you want a single host:
  ```bash
  oarsub -l host=1/core=4 -I
  ```
  - stress pinned cores `stress --cpu 5`
  - check with `htop -u $(whoami)`
  
#### More than one node
You will probably want to use more than one node on a given site. 
- For instance, how to reserve 2 nodes in an interactive mode ?
  ```bash
  oarsub -l host=2 -I 
  ```
- You will obtain a shell on the first node of the reservation. It is up to you to connect to the other nodes and distribute work among them. To list the nodes allocated use the variable `$OAR_FILE_NODES`.
  ```bash
  uniq $OAR_FILE_NODES
  ```
- By default, you can only connect to nodes that are part of your reservation, and only using the `oarsh` connector to go from one node to the other. The connector supports the same options as the classical `ssh` command use option `-t allow_classic_ssh`, so it can be used as a replacement for software expecting ssh. Read [tips & tricks ssh vs. oarsh](https://www.grid5000.fr/w/Advanced_OAR#oarsh_vs_ssh:_tips_and_tricks)

#### Interactive mode without shell
You may not want a job to open a shell or to run a script when the job starts, for example because you will use the reserved resources from a program whose lifecycle is longer than the job (and which will use the resources by connecting to the job).

One trick to achieve this is to run the job in passive mode with a long `sleep` command. One drawback of this method is that the job may terminate with status error if the sleep is killed. This can be a problem in some situations, eg. when using job dependencies.

Another solution is to use an advance reservation (see below) with a starting date very close in the future, or even with the current date and time. 

#### Other types of resources (GPU)
- To reserve only one GPU (with the associated CPU cores and share of memory) in interactive mode, run:
  ```
  oarsub -l gpu=1 -I
  ```
  Even if the node has several GPUs, this reservation will only be able to access a single one. It's a good practice if you only need one GPU: other users will be able to run jobs on the same node to access the other GPUs. Of course, if you need all GPUs of a node, you have the option to reserve the entire node which includes all its GPUs.
- To reserve several GPUs and ensure they are located in a single node, make sure to specify host=1:
  ```
  oarsub -l host=1/gpu=2 -I
  ```

- To reserve on a specific cluster use `-p` option. On Nantes site, we have 2 clusters `econome` and `ecotype`.
- To reserve a specific device, like a GPU. Available only on sites like Lyon, Lille, Grenble and Nancy. Here we reserve 1 GPU:
  ```bash
  oarsub -l gpu=1 -I
  ```


### Choosing a job duration

Of course, you might want to run a job for a different duration than one hour. The `-l` option allows you to pass a comma-separated list of parameters specifying the needed resources for the job, and walltime is a special resource defining the duration of your job:
```
oarsub -l host=1/core=2,walltime=0:30 -I
```
The walltime is the expected duration you envision to complete your work. Its format is [hour:min:sec|hour:min|hour]. For instance:
- `walltime=5` => 5 hours
- `walltime=1:22:00` => 1 hour and 22 minutes
- `walltime=0:03:30` => 3 minutes and 30 seconds

#### Extending the duration of a reservation
Provided that the resources are still available after your job, you can extend its duration (walltime) using e.g.:
- This will request to add one hour and a half to job #`12345`. 
  ```
  oarwalltime 12345 +1:30
  ```
  
### Tips & Tricks

To avoid unanticipated termination of your jobs in case of errors (terminal closed by mistake, network disconnection), you can either use tools such as `tmux` or `screen`, or you can also do it in 2 steps by using the job id associated to your reservation :
- reserve a node and run a process that does not end; here a linux `sleep` in an infinity loop. And of course do not use the option `-I`.
  ```bash
  oarsub "sleep infinity"
  ```
- connect to the node allocated with the job id
  ```bash
  oarsub -C <job id>
  ```

To force end your "infinite" allocation before the deadline falls, you can kill your job with `oardel <job id>`

### Requesting specific nodes or clusters

So far, all examples were letting OAR decide which resource to allocate to a job. It is possible to obtain finer-grained control on the allocated resources by using filters. 

```bash
oarsub -l host=1/gpu=1 -I -t exotic  
```

### Using OAR properties

- Nodes with Infiniband FDR interfaces @nancy site :
  ```bash 
  oarsub -p "ib='FDR'" -l host=5,walltime=00:02:00 -I -q besteffort
  ```
- Nodes with 2 GPUs @lille site :
  ```bash
  oarsub -p "gpu_count = 2" -l host=3,walltime=2 -I
  ```
- Nodes with a specific CPU model:
  ```bash
  oarsub -p "cputype = 'Intel Xeon E5-2630 v4'" -l host=3,walltime=2 -I  
  ```
  
### Job submission

```bash
cat<<EOF>$HOME/myjob.sh
#!/bin/zsh
#OAR -l host=1/core=1,walltime=00:05:00
#OAR -p cluster='econome'
hostname
EOF
chmod +x $HOME/myjob.sh
oarsub -S $HOME/myjob.sh
```

### Docker and Singularity

- run with singularity a docker running a `busybox` linux
  ```bash
  oarsub -l core=1 "/grid5000/code/bin/singularity exec docker://busybox uname -a"
  ```
- run a `lolcow`
  ```bash
  oarsub -l core=1 "/grid5000/code/bin/singularity run docker://godlovedc/lolcow"
  ```
- run a `gentoo` distro in a docker image
  ```bash
  oarsub -l core=1 "/grid5000/code/bin/singularity exec docker://gentoo/stage3-amd64 cat /etc/gentoo-release"
  ```
- run a tensorflow on a gpu cluster
  ```bash
  ssh nancy.g5k
  oarsub -q testing -p "cluster='gruss'" -l gpu=1,core=2 "singularity run --nv docker://tensorflow/tensorflow:latest-gpu"
  ```

<!--
https://www.grid5000.fr/w/TechTeam:UsagePolicyCheck
-->
