# Linux Multi-Distro Cluster Lab

A beginner-friendly Linux clustering project using multiple Linux distributions on virtual machines.

This project demonstrates how to build and manage a small cluster environment using:
- Ubuntu
- Oracle Linux
- Arch Linux

The purpose of this lab is to learn:
- Linux clustering concepts
- Kubernetes basics
- Docker/container orchestration
- Networking between Linux systems
- Cross-distribution compatibility

---

# Cluster Topology

```text
                +------------------+
                |   Ubuntu VM      |
                |  Kubernetes Main |
                |   192.168.1.10   |
                +---------+--------+
                          |
          -----------------------------------
          |                                 |
+---------+--------+             +----------+---------+
| Oracle Linux VM  |             |     Arch Linux VM |
| Kubernetes Worker|             | Kubernetes Worker |
|   192.168.1.11   |             |   192.168.1.12    |
+------------------+             +-------------------+
```

---

# What This Project Teaches

## Beginner Linux Skills
- Static IP configuration
- SSH connectivity
- Hostname management
- Package management across distros
- Service management using systemd

## Cluster Skills
- Kubernetes cluster setup
- Multi-node communication
- Worker node joining
- Container orchestration
- Cluster troubleshooting

## DevOps Skills
- Docker
- Kubernetes
- Linux networking
- Infrastructure management

---

# Requirements

## Hardware
Minimum:
- 3 Virtual Machines
- 2 CPU cores each
- 2GB RAM each
- 20GB disk each

Recommended:
- 4GB RAM per VM

---

# Operating Systems Used

| VM | Distribution | Purpose |
|----|--------------|---------|
| VM1 | Ubuntu Server | Kubernetes Master |
| VM2 | Oracle Linux | Worker Node |
| VM3 | Arch Linux | Worker Node |

---

# Network Configuration

Example setup:

| Hostname | IP Address |
|----------|-------------|
| master1 | 192.168.1.10 |
| worker1 | 192.168.1.11 |
| worker2 | 192.168.1.12 |

All VMs must:
- Be on the same subnet
- Ping each other successfully
- Have internet access
- Have SSH enabled

---

# Step 1 — Configure Hostnames

## Ubuntu
```bash
sudo hostnamectl set-hostname master1
```

## Oracle Linux
```bash
sudo hostnamectl set-hostname worker1
```

## Arch Linux
```bash
sudo hostnamectl set-hostname worker2
```

---

# Step 2 — Update Hosts File

Add this on ALL VMs:

```bash
sudo nano /etc/hosts
```

Add:

```text
192.168.1.10 master1
192.168.1.11 worker1
192.168.1.12 worker2
```

---

# Step 3 — Install Docker

## Ubuntu
```bash
sudo apt update
sudo apt install docker.io -y
```

## Oracle Linux
```bash
sudo dnf install docker -y
```

## Arch Linux
```bash
sudo pacman -S docker
```

Enable Docker:

```bash
sudo systemctl enable --now docker
```

---

# Step 4 — Install Kubernetes Tools

## Ubuntu
```bash
sudo apt install kubeadm kubelet kubectl -y
```

## Oracle Linux
```bash
sudo dnf install kubeadm kubelet kubectl -y
```

## Arch Linux
```bash
sudo pacman -S kubeadm kubelet kubectl
```

Enable kubelet:

```bash
sudo systemctl enable --now kubelet
```

---

# Step 5 — Initialize Kubernetes Master

Run ONLY on Ubuntu master node:

```bash
sudo kubeadm init
```

After completion, Kubernetes will generate a join command.

Example:

```bash
kubeadm join 192.168.1.10:6443 --token xxxxx \
--discovery-token-ca-cert-hash sha256:xxxxx
```

---

# Step 6 — Join Worker Nodes

Run the generated join command on:
- Oracle Linux VM
- Arch Linux VM

---

# Step 7 — Verify Cluster

On master node:

```bash
kubectl get nodes
```

Expected output:

```text
NAME       STATUS   ROLES           AGE
master1    Ready    control-plane   10m
worker1    Ready    <none>          5m
worker2    Ready    <none>          5m
```

---

# Testing The Cluster

Deploy nginx:

```bash
kubectl create deployment nginx --image=nginx
```

Check pods:

```bash
kubectl get pods -o wide
```

---

# Why Mixed Linux Distros?

This project demonstrates:
- Cross-platform Linux compatibility
- Kubernetes flexibility
- Real-world infrastructure diversity
- Linux administration differences

This setup is excellent for:
- Students
- Homelabs
- Beginners
- DevOps learners
- Linux enthusiasts

---

# Important Notes

## Production Recommendation
In production environments:
- Use the same Linux distribution
- Use identical kernel versions
- Use identical package versions

Mixed distributions are best suited for:
- Learning
- Labs
- Testing
- Development

---

# Future Improvements

Possible upgrades:
- Add HAProxy load balancer
- Add monitoring using Prometheus
- Add Grafana dashboards
- Add Helm
- Add Ceph storage
- Add CI/CD pipelines
- Add Ansible automation

---

# Useful Commands

## Check cluster nodes
```bash
kubectl get nodes
```

## Check pods
```bash
kubectl get pods -A
```

## Restart Docker
```bash
sudo systemctl restart docker
```

## Restart kubelet
```bash
sudo systemctl restart kubelet
```

---

# Technologies Used

- Linux
- Docker
- Kubernetes
- Ubuntu
- Oracle Linux
- Arch Linux

---

# Learning Goals

After completing this lab, you should understand:
- Linux clustering basics
- Kubernetes architecture
- Container orchestration
- Linux networking
- Multi-node environments
- Infrastructure management

---

# License

This project is open-source and free to use for learning purposes.

---

# Author

Created for Linux and DevOps beginners learning cluster systems.