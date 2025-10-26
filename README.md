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
