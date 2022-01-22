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
