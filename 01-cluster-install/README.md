# 🚀 Red Hat OpenShift Container Platform 4.21 UPI Installation on VMware vSphere ESXi

> A production-style implementation guide for deploying Red Hat OpenShift Container Platform (OCP) 4.21 using the User-Provisioned Infrastructure (UPI) installation method on VMware vSphere ESXi.

---

## 📖 Overview

This project documents the complete deployment of **Red Hat OpenShift Container Platform (OCP) 4.21** on **VMware vSphere ESXi** using the **User-Provisioned Infrastructure (UPI)** installation model.

Unlike Installer-Provisioned Infrastructure (IPI), the UPI deployment model requires manual preparation of the underlying infrastructure, including networking, DNS, load balancing, virtual machine provisioning, and ignition configuration. This provides complete control over the deployment process and closely reflects real-world enterprise environments.

This guide is based on an actual production-style lab deployment and includes real commands, screenshots, validation procedures, and implementation notes.

---

## 📑 Table of Contents

- [Overview](#-overview)
- [Objectives](#-objectives)
- [High-Level Architecture](#-high-level-architecture)
- [Lab Environment](#-lab-environment)
- [Deployment Workflow](#-deployment-workflow)
- [Installation Guide](#-installation-guide)
  - [Step 01: Bastion Host Preparation](#step-01-bastion-host-preparation)
  - [Step 02: DNS Configuration](#step-02-dns-configuration)
  - [Step 03: Configure API & Apps VIP](#step-03-configure-api--apps-vip)
  - [Step 04: HAProxy Installation & Configuration](#step-04-haproxy-installation--configuration)
  - [Step 05: Create OpenShift Installation Assets](#step-05-create-openshift-installation-assets)
  - [Step 06: Deploy RHCOS Virtual Machines](#step-06-deploy-rhcos-virtual-machines)
  - [Step 07: Inject Ignition Configuration](#step-07-inject-ignition-configuration)
  - [Step 08: Bootstrap Node Initialization](#step-08-bootstrap-node-initialization)
  - [Step 09: Bootstrap Completion](#step-09-bootstrap-completion)
  - [Step 10: Deploy Control Plane Nodes](#step-10-deploy-control-plane-nodes)
  - [Step 11: Deploy Worker Nodes](#step-11-deploy-worker-nodes)
  - [Step 12: Certificate Signing Request (CSR) Approval](#step-12-certificate-signing-request-csr-approval)
  - [Step 13: Verify Cluster Nodes](#step-13-verify-cluster-nodes)
  - [Step 14: Verify Cluster Operators](#step-14-verify-cluster-operators)
  - [Step 15: Verify Cluster Version](#step-15-verify-cluster-version)
  - [Step 16: Access OpenShift Web Console](#step-16-access-openshift-web-console)
- [Final Validation](#-final-validation)
- [Lessons Learned](#-lessons-learned)
- [References](#-references)

---

## 🎯 Objectives

- Deploy Red Hat OpenShift Container Platform 4.21 using the User-Provisioned Infrastructure (UPI) installation model.
- Configure VMware vSphere infrastructure for bootstrap, control plane, and worker nodes.
- Configure Windows Active Directory DNS and external HAProxy load balancing.
- Generate and inject Ignition configuration files.
- Deploy and validate a highly available OpenShift cluster.
- Document every deployment phase using commands, screenshots, validation checks, and production best practices.

---

## 🏗️ High-Level Architecture

```text
                           VMware vSphere ESXi
                                     │
        ┌────────────────────────────┴────────────────────────────┐
        │                                                         │
        ▼                                                         ▼
  Bastion Host                                            External HAProxy
        │                                                         │
        └────────────────────────────┬────────────────────────────┘
                                     │
                                     ▼
                             Bootstrap Node
                                     │
                                     ▼
                        Control Plane (3 Nodes)
                                     │
                                     ▼
                               Worker Nodes
                                     │
                                     ▼
               Red Hat OpenShift Container Platform 4.21
```

### Screenshot

> `images/01-lab-architecture.png`

---

## 🖥️ Lab Environment

| Component | Details |
|-----------|---------|
| OpenShift Version | 4.21.21 |
| Kubernetes Version | v1.34.x |
| Deployment Method | User-Provisioned Infrastructure (UPI) |
| Hypervisor | VMware vSphere ESXi |
| Operating System | Red Hat Enterprise Linux 9 |
| DNS | Windows Active Directory DNS |
| Load Balancer | HAProxy |
| Storage | TrueNAS CSI |
| Container Runtime | CRI-O |
| Network | Static IP Addressing |

---

## 📋 Deployment Workflow

```text
Bastion Host Preparation
        │
        ▼
DNS Configuration
        │
        ▼
API & Apps VIP Configuration
        │
        ▼
HAProxy Installation & Configuration
        │
        ▼
Create OpenShift Installation Assets
        │
        ▼
Deploy Bootstrap Node
        │
        ▼
Deploy Control Plane Nodes
        │
        ▼
Bootstrap Completion
        │
        ▼
Deploy Worker Nodes
        │
        ▼
Approve Certificate Signing Requests
        │
        ▼
Cluster Validation
        │
        ▼
Access OpenShift Web Console
```

---

## 🚀 Installation Guide

## Step 01: Bastion Host Preparation

### Objective

The bastion host serves as the administration node for the entire OpenShift installation. It is responsible for generating installation assets, managing the deployment process, interacting with the OpenShift cluster using the `oc` CLI, and hosting supporting services during installation.

Before starting the deployment, the bastion host must be properly configured with the required operating system packages, OpenShift installation binaries, network connectivity, and administrative privileges.

---

### Tasks Performed

- Installed Red Hat Enterprise Linux 9.
- Configured hostname and static IP address.
- Updated all operating system packages.
- Installed required utilities.
- Downloaded the OpenShift Installer.
- Downloaded the OpenShift CLI (`oc`).
- Configured SSH access.
- Verified internet connectivity.
- Prepared the working directory for cluster installation.

---

### Commands

#### Verify Hostname

```bash
hostnamectl
```

#### Verify Network Configuration

```bash
ip addr
```

#### Verify DNS Resolution

```bash
ping console.redhat.com
```

#### Verify OpenShift Client

```bash
oc version
```

#### Verify OpenShift Installer

```bash
openshift-install version
```

---

### Expected Output

The following conditions should be verified before proceeding.

| Validation | Status |
|------------|--------|
| Hostname configured | ✅ |
| Static IP configured | ✅ |
| Internet connectivity | ✅ |
| DNS resolution working | ✅ |
| OpenShift CLI installed | ✅ |
| OpenShift Installer installed | ✅ |

---

### Screenshot

> 📷 `images/02-bastion-host-preparation.png`

---

### Validation

The bastion host is considered ready when:

- The operating system is fully updated.
- Network connectivity is available.
- DNS resolution is functioning correctly.
- The OpenShift CLI is accessible.
- The OpenShift Installer version matches the target OpenShift release.
- The user can successfully execute administrative commands without errors.

---

### Production Notes

- Keep the OpenShift Installer (`openshift-install`) and OpenShift Client (`oc`) at the same version.
- Use a dedicated bastion host instead of performing the installation from a workstation.
- Synchronize the system time using NTP before starting the installation.
- Ensure passwordless SSH connectivity to all cluster nodes.
- Maintain a dedicated working directory to store installation assets and Ignition files.

---

### Lessons Learned

A properly prepared bastion host minimizes deployment issues and provides a consistent environment for cluster installation. Verifying all prerequisites before generating installation assets helps avoid installation failures later in the deployment process.


### Step 02: DNS Configuration

> `images/03-dns-configuration.png`

---

### Step 03: Configure API & Apps VIP

> `images/04-vip-configuration.png`

---

### Step 04: HAProxy Installation & Configuration

> `images/05-haproxy-installation.png`

---

### Step 05: Create OpenShift Installation Assets

> `images/06-installation-assets.png`

---

### Step 06: Deploy RHCOS Virtual Machines

> `images/07-rhcos-deployment.png`

---

### Step 07: Inject Ignition Configuration

> `images/08-ignition-configuration.png`

---

### Step 08: Bootstrap Node Initialization

> `images/09-bootstrap-node.png`

---

### Step 09: Bootstrap Completion

> `images/10-bootstrap-completion.png`

---

### Step 10: Deploy Control Plane Nodes

> `images/11-control-plane-nodes.png`

---

### Step 11: Deploy Worker Nodes

> `images/12-worker-nodes.png`

---

### Step 12: Certificate Signing Request (CSR) Approval

> `images/13-csr-approval.png`

---

### Step 13: Verify Cluster Nodes

> `images/14-cluster-nodes.png`

---

### Step 14: Verify Cluster Operators

> `images/15-cluster-operators.png`

---

### Step 15: Verify Cluster Version

> `images/16-cluster-version.png`

---

### Step 16: Access OpenShift Web Console

> `images/17-web-console.png`

---

## ✅ Final Validation

```bash
oc get nodes -o wide
oc get co
oc get clusterversion
oc get csr
oc whoami
oc whoami --show-console
```

**Screenshot**

> `images/18-final-validation.png`

---

## 📚 Lessons Learned

- Successfully deployed a production-style Red Hat OpenShift 4.21 UPI cluster.
- Configured enterprise networking using HAProxy and Windows Active Directory DNS.
- Deployed and validated bootstrap, control plane, and worker nodes.
- Verified overall cluster health and operator status.
- Built a reusable enterprise deployment methodology.

---

## 📖 References

- Red Hat OpenShift Container Platform Documentation
- OpenShift Installation Guide
- VMware vSphere Documentation
- HAProxy Documentation
- Red Hat Enterprise Linux Documentation
- Kubernetes Documentation
