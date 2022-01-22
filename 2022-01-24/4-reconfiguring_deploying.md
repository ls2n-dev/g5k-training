# Deploy your nodes, get root access and create your own experimental environment
[*hacked from source*](https://www.grid5000.fr/w/Getting_Started#Deploying_your_nodes_to_get_root_access_and_create_your_own_experimental_environment)

Using `oarsub` gives you access to resources configured in their default (standard) environment, with a set of software selected by the Grid'5000 team. You can use such an environment to run Java or [MPI programs](https://www.grid5000.fr/w/Run_MPI_On_Grid%275000), [boot virtual machines with KVM](https://www.grid5000.fr/w/Virtualization_on_Grid%275000), or [access a collection of scientific-related software](https://www.grid5000.fr/w/Environment_modules). However, you cannot deeply customize the software environment in a way or another.

Most Grid'5000 users use resources in a different, much more powerful way: they use [Kadeploy](http://kadeploy3.gforge.inria.fr) to re-install the nodes with their software environment for the duration of their experiment, using Grid'5000 as a Hardware-as-a-Service Cloud. This enables them to use a different Debian version, another Linux distribution, or even Windows, and get root access to install the software stack they need. 

Be root on the bare-metal machines on Grid'5000 with your (possibly customized environment). You have two options:
- get a standard environment (e.g. `oarsub -I`) and use `sudo-g5k`
- deploy an environment and be `root` on it

## Deploying nodes with builtin-env

### Available Environments
These environments are maintained by G5K staff. You can  ist all available environment in a site by using the following command line:
```
kaenv3 -l
```

### Deploy a environment
The deployment job type is required to allow deployment with *Kadeploy* by setting the flag `-t deploy`.

```bash
# add the -t deploy type
oarsub -I -l host=1 -t deploy

# note that we aren't on the first node of the reservation but
# on the frontend but connected to the job (OAR env variables are available)
kadeploy3 -e ubuntu2004-x64-min -k ~/.ssh/id_rsa.pub -f $OAR_FILE_NODES
```
- options
  ```bash
  # -e name of an env in the list of public env
  # -k key to push on the remote nodes (in /root/.ssh/authorized_keys)
  # -f file containing the list of host to deploy the env on
  ```
We start here a deployment of the `ubuntu1804-x64-min` image on that node (this takes 5 to 10 minutes): 


|:exclamation: IMPORTANT|
|:---|
|You should select at least a host exclusively and a walltime which should be a sum a minimum of *time of installation and estimation of your run of that node*|

### What’s an environment file ?:
An environment is described by a YAML document. To use the new image, it must be referred by an environment description, so that deploying that environment uses the new image. Note that the environment includes also other information such as the postinstall script and the kernel command line for instance, which can be changed independently from changing the environment image. 

```
frontend% kaenv3 -p ubuntu2004-x64-min -u deploy
---
name: ubuntu1804-x64-min
version: 2022010511
arch: x86_64
description: ubuntu 18.04 (bionic) for x64 - min
author: support-staff@lists.grid5000.fr
visibility: public
destructive: false
os: linux
image:
  file: server:///grid5000/images/ubuntu1804-x64-min-2022010511.tar.zst
  kind: tar
  compression: zstd
postinstalls:
- archive: server:///grid5000/postinstalls/g5k-postinstall.tgz
  compression: gzip
  script: g5k-postinstall --net netplan --disk-aliases
boot:
  kernel: "/vmlinuz"
  initrd: "/initrd.img"
  kernel_params: ''
filesystem: ext4
partition_type: 131
multipart: false
```

## Deploying node with a custom environment

### Create your env
Build the `.zstd` file.

You have two options:
- Deploy an env, modify it and save it
  ```bash
  frontend% tgz-g5k -m <node> -z -f ~/path_to_myimage.tar.zst
  ```
- Build it from scratch (see https://www.grid5000.fr/w/Environments_creation_using_Kameleon_and_Puppet)
        This is how the public environments are built in a reproducible way

### Deploy it
```
kadeploy3 -a mydescription.env -k ~/.ssh/id_rsa.pub -f $OAR_FILE_NODES
# options
# -a points to a custom description of environment with your own image/postinstalls/boot options...
```


|:memo: Read further|
|:---|
|https://www.grid5000.fr/w/Advanced_Kadeploy<br>https://www.grid5000.fr/w/Environment_creation|
