# Allocating and accessing resources with OAR

OAR is the **resources and jobs management system** (a.k.a batch manager) used in Grid'5000, just like in traditional HPC centers (commonly with SLURM, PBS, etc.). However, settings and rules of OAR that are configured in Grid'5000 slightly differ from traditional batch manager setups in HPC centers, in order to match the requirements for an experimentation testbed and **not for production use**! 

In Grid'5000 the **smallest unit of resource managed by OAR is the core (cpu core)**, but by default a OAR job reserves a host (physical computer including all its cpus and cores, and possibly gpus). Hence, what OAR calls nodes are hosts (physical machines). In the `oarsub` resource request (`-l` arguments), nodes is an alias for host, so both are equivalent. But prefer using host for consistency with other argumnents and other tools that expose host not nodes. 

[OAR Documentation here](http://oar.imag.fr/documentation)

## Some (control) commands

- `oarsub`: submit jobs
- `oarstat`: check job status
   - `oarstat -u [-f]`:  show your present and future jobs. Optionaly add more information
   - `oarstat -j <jobid> [-f]`: show the status of a specific job. Optionaly add more information 
   - `oarstat -j 123456 --json`: same but with a json output. Can be piped to `jq`: ` oarstat -j 123456 --json | jq '.[].command'`
- `oardel`: delete a job

## Basic command in an interactive mode

- To reserve a single host (one node) for one hour, in an interactive mode (`-I` option), just do:
  ```bash
  oarsub -I -q besteffort [ --project ... ]
  ```
  As soon as the resource becomes available, you will be directly connected to the reserved resource with an interactive shell, as indicated by the shell prompt, and you can run commands on the node: `lscpu` or on the web with [Gantt diagram / Monika](https://www.grid5000.fr/w/Status#Resources_reservations_.28OAR.29_status).

- To reserve only part of a node. For e.g. only 2 CPU cores for `2 minutes` on the same host, run:
  ```bash
  oarsub -q besteffort -l host=1/core=2,walltime=00:02:00 -I
  ```

  - stress pinned cores `stress --cpu`
  - checkout with `htop -u ...`

- To reserve on a specific cluster use `-p` option. On Nantes site, we have 2 clusters `econome` and `ecotype`.
- To reserve a specific device, like a GPU. Available only on sites like Lyon, Lille, Grenble and Nancy. Here we reserve 1 GPU:
  ```bash
  oarsub -l gpu=1 -I
  ```
  
**Before the walltime ends, if you logout from your active session in this mode your reservation will be ended immediately**

## Batch command

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


### Working with more than one node
You will probably want to use more than one node on a given site. 

For instance, how to reserve 2 nodes in an interactive mode ?

```bash
oarsub -l host=2 -I  [ -t allow_classic_ssh ]
```

You will obtain a shell on the first node of the reservation. It is up to you to connect to the other nodes and distribute work among them. To list the nodes allocated use the variable `$OAR_FILE_NODES`.

```bash
uniq $OAR_FILE_NODES
```

By default, you can only connect to nodes that are part of your reservation, and only using the `oarsh` connector to go from one node to the other. The connector supports the same options as the classical ssh command, with the option `-t allow_classic_ssh`, so it can be used as a replacement for software expecting ssh.

## Requesting specific nodes or clusters

So far, all examples were letting OAR decide which resource to allocate to a job. It is possible to obtain finer-grained control on the allocated resources by using filters. 

```bash
oarsub -l host=1/gpu=1 -I -t exotic  
```

## Using OAR properties

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
  
## Job submission

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

## Docker and Singularity

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
