# 🚀 OpenShift 4.21 UPI Cluster Installation on VMware vSphere

> Production-style deployment of Red Hat OpenShift Container Platform 4.21 using User-Provisioned Infrastructure (UPI) on VMware vSphere ESXi.

---

# 📖 Overview

This document provides a complete walkthrough for deploying a highly available Red Hat OpenShift 4.21 cluster using the User-Provisioned Infrastructure (UPI) installation method.

The deployment was performed on VMware vSphere ESXi with external HAProxy, Windows Active Directory DNS, static IP addressing, and manually injected Ignition configurations. Every stage of the deployment was validated to ensure the cluster was ready for enterprise workloads.

---

# 🎯 Objectives

- Deploy OpenShift 4.21 using the UPI installation model.
- Configure VMware virtual machines for bootstrap, control plane, and worker nodes.
- Configure external DNS and HAProxy load balancing.
- Generate and inject Ignition files.
- Validate cluster health after installation.
- Document the complete installation process with screenshots and verification commands.

---

# 🏗 Lab Architecture

```text
                    VMware vSphere ESXi
                            │
        ┌───────────────────┴───────────────────┐
        │                                       │
        ▼                                       ▼
   Bootstrap Node                    Control Plane Nodes
                                            │
                                            ▼
                                     Worker Nodes
                                            │
                                            ▼
                               OpenShift 4.21 Cluster
```

> 📷 **Screenshot Placeholder**
>
> `images/01-lab-architecture.png`

---

# 🖥 Lab Environment

| Component | Details |
|-----------|---------|
| OpenShift Version | 4.21.21 |
| Kubernetes | v1.34.x |
| Deployment Model | User-Provisioned Infrastructure (UPI) |
| Hypervisor | VMware vSphere ESXi |
| DNS | Windows Active Directory DNS |
| Load Balancer | External HAProxy |
| Network | Static IP |
| Storage | TrueNAS CSI |
| Platform | Red Hat Enterprise Linux |

---

# 📋 Deployment Workflow

```text
Bastion Host Preparation
        │
        ▼
DNS Configuration
        │
        ▼
HAProxy Configuration
        │
        ▼
Create Installation Assets
        │
        ▼
Deploy Bootstrap Node
        │
        ▼
Deploy Master Nodes
        │
        ▼
Bootstrap Complete
        │
        ▼
Deploy Worker Nodes
        │
        ▼
Approve CSR Requests
        │
        ▼
Cluster Validation
        │
        ▼
OpenShift Console Ready
```

---

# 🚀 Installation Steps

## Step 1 — Bastion Host Preparation

### Description

Prepare the bastion host with all required tools and verify the OpenShift installer version.

### Commands

```bash
hostnamectl
ip a
oc version
openshift-install version
```

### Expected Result

The bastion host is ready with the correct OpenShift client and installer versions.

📷 **Screenshot Placeholder**

`images/02-environment-preparation.png`

---

## Step 2 — DNS Configuration

### Description

Create and validate all required DNS records before deploying any virtual machines.

### Commands

```bash
nslookup api.ocp4.example.com
nslookup api-int.ocp4.example.com
nslookup bootstrap.ocp4.example.com
```

### Expected Result

All DNS records resolve successfully.

📷 **Screenshot Placeholder**

`images/03-dns-validation.png`

---

## Step 3 — Configure API & Apps VIP

### Commands

```bash
ip addr show
```

📷 **Screenshot Placeholder**

`images/04-vip-configuration.png`

---

## Step 4 — HAProxy Installation & Configuration

### Commands

```bash
systemctl status haproxy
haproxy -c -f /etc/haproxy/haproxy.cfg
```

📷 **Screenshot Placeholder**

`images/05-haproxy-installation.png`

---

## Step 5 — Create Installation Assets

### Commands

```bash
openshift-install create manifests
openshift-install create ignition-configs
```

📷 **Screenshot Placeholder**

`images/06-install-assets.png`

---

## Step 6 — Deploy RHCOS Virtual Machines

📷 **Screenshot Placeholder**

`images/07-rhcos-template.png`

---

## Step 7 — Inject Ignition Configuration

📷 **Screenshot Placeholder**

`images/08-ignition-configuration.png`

---

## Step 8 — Bootstrap Node Initialization

### Commands

```bash
journalctl -b -f
```

📷 **Screenshot Placeholder**

`images/09-bootstrap.png`

---

## Step 9 — Bootstrap Complete

### Commands

```bash
openshift-install wait-for bootstrap-complete
```

📷 **Screenshot Placeholder**

`images/10-bootstrap-complete.png`

---

## Step 10 — Power On Worker Nodes

📷 **Screenshot Placeholder**

`images/11-worker-startup.png`

---

## Step 11 — CSR Approval

### Commands

```bash
oc get csr
oc adm certificate approve <csr-name>
```

📷 **Screenshot Placeholder**

`images/12-csr-approval.png`

---

## Step 12 — Verify Cluster Nodes

### Commands

```bash
oc get nodes
```

📷 **Screenshot Placeholder**

`images/13-cluster-nodes.png`

---

## Step 13 — Verify Cluster Operators

### Commands

```bash
oc get co
```

📷 **Screenshot Placeholder**

`images/14-cluster-operators.png`

---

## Step 14 — Verify Cluster Version

### Commands

```bash
oc get clusterversion
```

📷 **Screenshot Placeholder**

`images/15-cluster-version.png`

---

## Step 15 — Access OpenShift Console

### Commands

```bash
oc whoami --show-console
```

📷 **Screenshot Placeholder**

`images/16-web-console.png`

---

# ✅ Final Validation

```bash
oc get nodes
oc get co
oc get clusterversion
oc whoami
```

📷 **Screenshot Placeholder**

`images/17-final-validation.png`

---

# 📚 Lessons Learned

- Successfully deployed a production-style OpenShift UPI cluster on VMware vSphere.
- Configured external HAProxy and Windows AD DNS.
- Generated and applied Ignition configurations.
- Validated bootstrap, control plane, and worker node deployment.
- Verified cluster health and readiness for enterprise workloads.

---

# 📖 References

- Red Hat OpenShift Documentation
- VMware vSphere Documentation
- Kubernetes Documentation
