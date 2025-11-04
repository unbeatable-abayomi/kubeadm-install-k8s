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

#Run the command below to confirm hostname
 hostname
```

### Step 3: To configure the IPV4 bridge on all nodes, execute the following commands on each node.
```bash
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

sudo modprobe overlay
sudo modprobe br_netfilter

# sysctl params required by setup, params persist across reboots
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF

# Apply sysctl params without reboot
sudo sysctl --system
```

### Step 4: Next, we have to ensure that we can download and install packages from the internet securely.
```bash
# Run the command below on all nodes
sudo apt-get install -y apt-transport-https ca-certificates curl

# Next, we have to create a directory where we'll store a special key that verifies the authenticity of Kubernetes packages.
sudo mkdir /etc/apt/keyrings
```
### Step 5: Let’s fetch the public key from Google and store it in the folder we created in the previous step
```bash
# Run below command on all nodes
   curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.31/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

sudo chmod 644 /etc/apt/keyrings/kubernetes-apt-keyring.gpg

# Next, we need to tell the apt package manager where to find Kubernetes packages for downloading.
echo "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.31/deb/ /" | sudo tee /etc/apt/sources.list.d/kubernetes.list

#Next let’s refresh the apt package index to see new items, Run below command on all nodes
sudo apt-get update

```