# LFS-guide 

Dummies guide to LFS

### disclaimer
please refer to the [official Linux From Scratch guide](https://www.linuxfromscratch.org/lfs/view/stable/index.html). This guide is just an overview short guideline refer to the LFS document for more

## BOOT
![Boot](./demo_boot.gif)

## Neofetch :)
![Neofetch](./neofetch.gif)

## STEP 1 set up the environment where we will use to build LFS. I'll use Ubuntu 24.04.3 running on a VM you can use other distro, or do this outside of VMs

### My VM Spec
- VMware workstation 17
- Ubuntu 24.04.3
- RAM: 8 gb
- Processor: 1 CPU
- 80gb of storage

## STEP 2 Preparing your base OS for building LFS

### basic requirement 
- [install required packages](https://www.linuxfromscratch.org/lfs/view/10.1-rc1/chapter02/hostreqs.html)
```shell
sudo apt install bison gawk m4 texinfo 
sudo ln -sf bash /usr/bin/sh
```
- you could also run a version check after install all the packages using this [version-check.sh](https://www.linuxfromscratch.org/lfs/view/10.1-rc1/chapter02/hostreqs.html)

### Create new partition

we will be using the fdisk to do this is you're not similar with the program i recommand this [comprehensive guide](https://www.howtogeek.com/106873/how-to-use-fdisk-to-manage-partitions-on-linux/) we will start by listing the partition we have and then start creating a new partition for building LFS.

- use -l to list the partition
```shell
sudo fdisk -l /dev/sbd
```
- create a GPT partition
```shell
fdisk /dev/sdb
```

