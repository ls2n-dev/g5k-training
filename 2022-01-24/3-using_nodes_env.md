# Using nodes in the default environment

When you run `oarsub`, you gain access to physical nodes with a default (standard) software environment. This is a Debian-based system that is regularly updated by the technical team. It contains many pre-installed software packages installed in the default environment like: `Git, GCC, Python, Pip, Numpy, Ruby, Java...`

*Play on a node by yourself*

## Home directory and `/tmp`

On each node, we have:
- the home directory `/home` is a network filesystem (NFS) and data in this directory is not actually stored on the node itself, it is stored on a storage server managed by the Grid'5000 team. In particular, it means that all reserved nodes share the same home directory, and it is also shared with the site frontend. For example, you can compile or install software in your home (possibly using pip, virtualenv), and it will be usable on all your nodes. 
- the `/tmp` directory is stored on a local disk of the node. Use this directory if you need to access data locally.

|:memo: Take note of this|
|:---|
|The home directory is only **shared within a site**. Two nodes from different sites will not have access to the same home.|

## Becoming root with `sudo-g5k`

On HPC clusters, users typically don't have root access. However, Grid'5000 allows more flexibility: if you need to install additional system packages or to customize the system, it is possible to become (sudo) root. The tool to do this is called `sudo-g5k`.

|:point_up: Remember not to forget|
|:---|
|Using `sudo-g5k` has a cost for the platform: at the end of your job, the node needs to be completely reinstalled so that it is clean for the next user. So it is best to avoid running `sudo-g5k` in very short jobs (less than a hour for instance). The fact that you need to reinstall new packages, `sudo-g5k` will not allow you to perform those tasks if you haven't allocated an entire node, so e.g. `-l core=1` will not work. |

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

## Additional disks and storage

Some nodes have additional local disks (not on Nantes site), see [Hardware/Storage](https://www.grid5000.fr/w/Hardware#Storage) for a list of available disks for each cluster. There are two ways to access these local disks:
- on some clusters, local disks need to be reserved to be accessible. See [Disk reservation](https://www.grid5000.fr/w/Disk_reservation) for a list of these clusters and for documentation on the reservation process.
- on other clusters, local disks can be used directly. In this case, jump directly to [Using local disks](https://www.grid5000.fr/w/Disk_reservation#Using_local_disks_once_connected_on_the_nodes).

In both cases, the disks are simply provided as raw devices, and it is the responsibility of the user to partition them and create a filesystem. Note that there may still be partitions and filesystems present from a previous job. In addition to local disks, more [storage options](https://www.grid5000.fr/w/Storage) are available. 

|:exclamation: Important |
|:---|
|Grid'5000 does NOT have a BACKUP service for the storage it provides: it is your own responsibility to save important data outside Grid'5000 (or at least to copy data to several Grid'5000 sites in order to increase redundancy). |

