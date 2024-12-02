# Redwood User Guide

This guide is intended for users of Redwood (redwood.stanford.edu). If
you're a new user, please follow the steps below for first-time
setup. The rest of the guide describes how to use the machine.

## First-Time Setup

Please follow these steps when you first set up your Redwood account.

### Getting Access

To get access to Redwood, email `action@cs.stanford.edu` and CC
Rohan (`rohany@cs.stanford.edu`) and Fred (`kjolstad@stanford.edu`).

### How to SSH

```bash
ssh <cs-username>@redwood.stanford.edu
```

### Coordinate on #compiler-group-machine-users 

Coordinate with other users and ask questions on #compiler-group-machine-users.

## Etiquette

Redwood is a shared research machine. This machine is intended for
GPU related research and debugging, with fast turn around times. It
is not intended for long-running jobs (like ML training) unless with
explicit coordination with other users of the machine.

## Filesystems


Currently, the following filesystems are accessible from Redwood:

* AFS (at /afs/cs.stanford.edu/). Your home directory is present there.
* Scratch (at /scratch/). Store most objects here.

When starting on the machine, please ask an admin (Rohan) to make a directory
on the scratch disk for you.

## Using the Machine

Some things to keep in mind while using the machine:

### 1. Module System

We provide a very minimal module system. Currenty, the only
provided modules are:

* CUDA
* Go

Modules can be loaded with
```bash
module load <module>/<version>
```

Available modules can be seen with
```bash
module avail
```

## Administration

How to...

### 1. Upgrade CUDA Driver

Contact `action@cs`. They installed the CUDA driver originally on the
`g000*` nodes, and know how to upgrade it.

For posterity, here is the upgrade procedure used (as of
2022-09-15)&mdash;but you can let the admins do this:

```
wget https://us.download.nvidia.com/XFree86/Linux-x86_64/515.65.01/NVIDIA-Linux-x86_64-515.65.01.run
systemctl stop nvidia-persistenced.service
chmod +x NVIDIA-Linux-x86_64-515.65.01.run
./NVIDIA-Linux-x86_64-515.65.01.run --uninstall
./NVIDIA-Linux-x86_64-515.65.01.run -q -a -n -X -s
systemctl start nvidia-persistenced.service
```

### 2. Install CUDA Toolkit

We can do this ourselves. Note: for the driver, see above.

```bash
cd admin/cuda
./install_cuda_toolkit.sh
./install_cudnn.sh # note: requires download (see script)
cd ../modules
./setup_modules.sh
```

### 3. Upgrade Linux Kernel (or System Software)

We can do this ourselves, but watch out for potential upgrade hazards
(e.g., GCC minor version updates, Linux kernel upgrades).

```bash
sudo apt update
sudo apt upgrade
sudo reboot
```

**Important:** check the status of the NVIDIA driver after this. If
`nvidia-smi` breaks, see above.
