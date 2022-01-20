# Deploy your nodes, get root access and create your own experimental environment
[*source*](https://www.grid5000.fr/w/Getting_Started#Deploying_your_nodes_to_get_root_access_and_create_your_own_experimental_environment)


Most Grid'5000 users use resources in a different, much more powerful way: they use Kadeploy to re-install the nodes with their software environment for the duration of their experiment, using Grid'5000 as a Hardware-as-a-Service Cloud. 
This enables them to use a different Debian version, another Linux distribution, or even Windows, and get root access to install the software stack they need. 

## Deploying nodes with Kadeploy

### Reserve one node 
The deployment job type is required to allow deployment with *Kadeploy*

```bash
oarsub -I -l host=1,walltime=00:10:00 -t deploy
```

### Find your image

You can also list all available environment in a site by using the following command line:
```
kaenv3 -l
```

```bash
...
ubuntu1804-x64-min    2021090715 deploy      ubuntu 18.04 (bionic) for x64 - min
...
```

Select the one you want to deploy, copy the first column.

### Start the deployment
We start here a deployment of the `ubuntu1804-x64-min` image on that node (this takes 5 to 10 minutes): 

```bash
kadeploy3 -f $OAR_NODE_FILE -e ubuntu1804-x64-min -k
```

### Connect to the new server

```bash
ssh root@<new-machine>
```
