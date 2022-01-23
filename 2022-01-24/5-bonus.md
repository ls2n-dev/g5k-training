# Using efficiently Grid'5000
Until now you have been logging, and submitting jobs manually to Grid'5000. This way of doing is convenient for learning, prototyping, and exploring ideas. But it may quickly become tedious when it comes to performing a set of experiments on a daily basis. In order to be more efficient and user-friendly, Grid'5000 also support more convenient ways of submitting jobs, such as API requests and computational notebooks. 

## Using API 

Until 2009, Grid'5000 was mainly accessed and operated via shell commands executed from frontend machines. To ease scripting and provide better access to the functionalities of the instrument (Grid'5000), an API has been developed on top of most of the Grid'5000 tools by the development team and is available to users since November 2009. 

**What Is an Application Programming Interface (API)?**
> Think of an API like a menu in a restaurant. The menu provides a list of dishes you can order, along with a description of each dish. When you specify what menu items you want, the restaurant’s kitchen does the work and provides you with some finished dishes. You don’t know exactly how the restaurant prepares that food, and you don’t really need to.

Similarly, an API lists a bunch of operations that developers can use, along with a description of what they do. The developer doesn’t necessarily need to know how, for example, an operating system builds and presents a “Save As” dialog box. They just need to know that it’s available for use in their app.

This isn’t a perfect metaphor, as developers may have to provide their own data to the API to get the results, so perhaps it’s more like a fancy restaurant where you can provide some of your own ingredients the kitchen will work with.

|:point_up: Just remember 2 things about API|
|:---|
| <ul><li>APIs Make Life Easier for Developers (find what you need and apply it)</li><li>APIs Control Access to Resources (with a restricted policy of usage)</li></ul>|

- https://grid5000.fr/w/API
- https://grid5000.fr/w/API_tutorial

## Notebooks
Computational notebooks such as Jupyter notebooks can be useful tools to drive or instrument experiments on Grid'5000. 

[*more information*](https://grid5000.fr/w/Notebooks)

## Virtual Private Network (VPN) 

It allows you to connect your workstation or personal computer to Grid'5000 network (direct connection to site frontend), while preserving security. When connected to Grid'5000 VPN, your computer will be "inside" the Grid'5000 network, thus it won't be required to perform several SSH hops or tunnels to access Grid'5000 nodes, since direct connections are possible. 

To start using Grid'5000 VPN, you first need to get a certificate:
1. Go to your account management page, in the "My account" tab and go to "VPN Certificates" on the left.
2. If you do not have a certificate yet, click on "Create new certificate".
   - To generate a new certificate click on "Create with Passphrase" (recommended). 
   - If you already generated a certificate by yourself, click on "Create from public key", paste your public key in the text field and finally "Sign".
3. Your certificate appears in the list. Click on "Zip file" to download an archive which includes the certificates and an OpenVPN configuration file.
4. Extract the archive content in your workstation. Please choose a secure place to store those files: an attacker could use them to steal your identity in Grid'5000.

Connect to site frontend to test if you are using the VPN:
```
ssh <login-g5k>@frontend.lyon.grid5000.fr
```

[*more information*](https://www.grid5000.fr/w/VPN)

## Access a collection of scientific-related software.

Like in the HPC system, Grid'5000 provides a set of software (mainly scientific-related) using [Environment modules](https://www.grid5000.fr/w/Environment_modules), thanks to the [module command line tool](http://modules.sourceforge.net). They are available from Grid5000 frontends or from the cluster's nodes (only on standard, big, and nfs environment if deployment is used). 

- Create a reservation with `oarsub`
- source the configuration before
  ```bash
  source /etc/profile.d/lmod.sh
  ```
- run `module avail` to see the installed libraries and software packages in Grid'5000.
  - `module list` to list your loaded soft/libs
  - `module load <app/lib>`
  - `module purge` to unload all loaded soft/lib
- see [module documentation here](https://modules.readthedocs.io/en/latest/module.html) or use `man module`

[*more information*](https://www.grid5000.fr/w/Environment_modules "Environment modules")

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

## Other Specific Usages
-   [Run MPI programs](https://www.grid5000.fr/w/Run_MPI_On_Grid%275000 "Run MPI On Grid'5000"), possibly taking advantage of several nodes
-   [Use GPUs with CUDA or AMD ROCm / HIP](https://www.grid5000.fr/w/Accelerators_on_Grid5000 "Accelerators on Grid5000")
-   [Install packages with Guix](https://www.grid5000.fr/w/Guix "Guix")
-   [Run containers with Singularity](https://www.grid5000.fr/w/Singularity "Singularity")
-   [Boot virtual machines with KVM](https://www.grid5000.fr/w/Virtualization_on_Grid%275000 "Virtualization on Grid'5000")

