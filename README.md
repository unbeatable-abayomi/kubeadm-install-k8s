#  Kubernetes 1.31 Cluster Setup on Ubuntu 22.04 LTS Using Kubeadm
This guide provides step-by-step instructions to set up a Kubernetes 1.31 cluster on Ubuntu 22.04 LTS using Kubeadm.

## Prerequisites

- Ubuntu 22.04 LTS installed on all nodes.
- Access to the internet.
- User with `sudo` privileges.

## Step-by-Step Installation

### Step 1: Disable Swap on All Nodes and Confirm

```bash
sudo swapoff -a


sudo sed -i '/swap/s/^/#/' /etc/fstab


sudo swapon --show
```
### Step 2: Change the hostname on your VM's or node
```bash
 # ssh into your VMs or just run the command in the terminal of each VM or node
 # Set the hostname of my 1st Node to k8s-master from the terminal of the node
 sudo hostnamectl set-hostname "k8s-master"

 # Set the hostname of my 2nd Node to k8s-worker from the terminal of the node
 sudo hostnamectl set-hostname "k8s-worker"

 #Run the command below should change the hostname on the terminal
 exec bash

 hostname
```