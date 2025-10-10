# LFS-guide 

Dummies guide to LFS

### disclaimer
please refer to the [official Linux From Scratch guide](https://www.linuxfromscratch.org/lfs/view/stable/index.html). This guide is just an overview short guideline refer to the LFS document for more

## STEP 1 set up the environment where we will use to build LFS.
 I'll use Ubuntu 24.04.3 running on a VM you can use other distro, or do this outside of VMs

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
sudo fdisk -l /dev/sda
```
- create a GPT partition
```shell
fdisk /dev/sda
```
- after entering fdisk use these command to create a GPT partition

    - g : insert func
    - n : insert func
    - p : insert func
    - w : insert func

- Create file system on the partition we just created 

    - format the partition to ext4 using ```mkfs```
    ```shell
    sudo mkfs -v -t ext4 <you new partition name>
    ```
- Set the $LFS Variable

```shell
export LFS=mnt/lfs
```
- mount the new partition
```shell
sudo -E mkdir -pv $LFS
sudo -E mount -v -t ext4 <you new partition name> $LFS
```

## STEP 3 Packages and Patches

- create a dir for our packages
```shell
mkdir -v $LFS/sources
chmod -v a+wt $LFS/sources #all write permission
```
- download packages for our LFS build 
```shell
wget https://www.linuxfromscratch.org/lfs/view/stable/wget-list-sysv
wget --input-file=wget-list-sysv --continue --directory-prefix=$LFS/sources
```

## STEP 4 Final Prep
after this we can start building our LFS

- create a limited dic layout in the lfs filesystem
using this command to create required directory
```shell
mkdir -pv $LFS/{etc,var} $LFS/usr/{bin,lib,sbin}

for i in bin lib sbin; do
  ln -sv usr/$i $LFS/$i
done

case $(uname -m) in
  x86_64) mkdir -pv $LFS/lib64 ;;
esac
```
```shell
mkdir -pv $LFS/tools
```

- add LFS user
to avoid logging in as ```root``` and destroy everything while we at it we gonna make a new user you can do this by
```shell
groupadd lfs <you can use your own desire usrname>
useradd -s /bin/bash -g lfs -m -k /dev/null lfs
```
- grant ```new user``` full access to all $LFS directories

```shell
chown -v lfs $LFS/{usr{,/*},var,etc,tools}
case $(uname -m) in
  x86_64) chown -v lfs $LFS/lib64 ;;
esac
```
- start a shell running as the new user we just create

```shell
su - lfs
```
- setting up the environment
we will set up a working environment by creating two new startup file for ```bash``` shell. create a new ```.bash_profile```:

```shell
cat > ~/.bashrc << "EOF"
set +h
umask 022
LFS=/mnt/lfs
LC_ALL=POSIX
LFS_TGT=$(uname -m)-lfs-linux-gnu
PATH=/usr/bin
if [ ! -L /bin ]; then PATH=/bin:$PATH; fi
PATH=$LFS/tools/bin:$PATH
CONFIG_SITE=$LFS/usr/share/config.site
export LFS LC_ALL LFS_TGT PATH CONFIG_SITE
EOF
```
## STEP 5. Compiling a cross-toolchain

https://www.linuxfromscratch.org/lfs/view/stable/chapter05/introduction.html

## STEP 6. Cross Compiling Temporary tools 

https://www.linuxfromscratch.org/lfs/view/stable/chapter06/introduction.html

## STEP 7. Entering Chroot and Building Additional Temporary Tools

https://www.linuxfromscratch.org/lfs/view/stable/chapter07/introduction.html

#