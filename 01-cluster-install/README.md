# OpenShift Cluster Installation

## Objective

Deploy a production-style Red Hat OpenShift Container Platform 4.21 cluster on VMware vSphere using the User Provisioned Infrastructure (UPI) method.

---

# Environment

| Component | Value |
|-----------|-------|
| Platform | VMware vSphere ESXi |
| Installation Method | UPI |
| OpenShift Version | 4.21.x |
| Kubernetes Version | v1.34.x |
| Control Plane | 3 Masters |
| Worker Nodes | 6 Workers |
| Bastion Host | RHEL 9 |
| DNS | Windows Server 2019 |
| Storage | TrueNAS SCALE |
| Container Runtime | CRI-O |

---

# Architecture

```
                    VMware ESXi
                         │
        ┌──────────────────────────────────┐
        │                                  │
   Bastion Host                       Windows DNS
        │
        │
  OpenShift Cluster
        │
 ┌──────┴──────────────┐
 │                     │
Masters (3)       Workers (6)
 │                     │
 └──────────────┬──────┘
                │
        OpenShift Platform
```

---

# Installation Workflow

- VMware ESXi Infrastructure Preparation
- Bastion Host Configuration
- DNS Configuration
- Download OpenShift Installer
- Generate Ignition Files
- Create Bootstrap VM
- Create Master Nodes
- Create Worker Nodes
- CSR Approval
- Cluster Installation Completion
- Worker Node Scaling
- Health Verification

---

# Verification Commands

## Cluster Version

```bash
oc get clusterversion
```

---

## Cluster Operators

```bash
oc get co
```

---

## Cluster Nodes

```bash
oc get nodes
```

---

## Expected Result

- Cluster Installation Successful
- All Masters Ready
- All Workers Ready
- All Operators Available
- Cluster Version Healthy

---

# Screenshots

## 1. VMware Infrastructure

OpenShift virtual machines running on VMware ESXi.

![VMware Infrastructure](images/vmware-esxi-vms.png)

---

## 2. Cluster Nodes

All control plane and worker nodes are in Ready state.

```bash
oc get nodes
```

![Cluster Nodes](images/oc-get-nodes.png)

---

## 3. Cluster Operators

All cluster operators are healthy.

```bash
oc get co
```

![Cluster Operators](images/oc-get-co.png)

---

## 4. Cluster Version

Cluster successfully upgraded and available.

```bash
oc get clusterversion
```

![Cluster Version](images/oc-get-clusterversion.png)

---

## 5. OpenShift Web Console

Cluster overview from the OpenShift Web Console.

![OpenShift Web Console](images/web-console-home.png)

---

# Outcome

Successfully deployed a production-grade Red Hat OpenShift 4.21 cluster on VMware ESXi using the UPI installation method with:

- 3 Control Plane Nodes
- 6 Worker Nodes
- Bastion Host
- Windows DNS
- Healthy Cluster Operators
- Production-ready Kubernetes Platform

---

# Technologies Used

- Red Hat OpenShift
- Kubernetes
- VMware ESXi
- RHEL 9
- CRI-O
- CoreOS
- Ignition
- Bash
- SSH

---

# Next Lab

➡️ **02-worker-node**

Adding a new worker node to the production OpenShift cluster.
