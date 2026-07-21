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
Windows AD DNS
        │
        │
Bastion Host ───── HAProxy
        │
        │
VMware ESXi Cluster
        │
 ┌───────────────┐
 │ Bootstrap VM  │
 └───────────────┘
        │
 ┌───────────────┐
 │ Master-0      │
 │ Master-1      │
 │ Master-2      │
 └───────────────┘
        │
 ┌───────────────┐
 │ Worker-0      │
 │ Worker-1      │
 └───────────────┘
        │
   OpenShift 4.21
```


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

Prepare the bastion host by installing the required packages, enabling time synchronization, and installing the OpenShift CLI (`oc`) and installer (`openshift-install`).

---

### Install Required Packages

```bash
sudo dnf update -y

sudo dnf install -y \
wget \
curl \
tar \
git \
jq \
haproxy \
bind-utils \
chrony \
python3
```

### Enable Time Synchronization

```bash
sudo systemctl enable --now chronyd

chronyc sources -v

timedatectl
```

### 📷 Screenshot

![Bastion Host Preparation - Required Packages](images/02-bastion-packages.png)

> **Figure 2.** Required packages successfully installed on the bastion host.
---

### Download OpenShift Client & Installer

Download the **OpenShift Client (`oc`)** and **OpenShift Installer (`openshift-install`)** binaries from the official Red Hat mirror. Ensure that both binaries match the target OpenShift cluster version.

> **Note:** Download the OpenShift Client and Installer versions that match your target OpenShift cluster version.

### Commands

```bash
cd /tmp

wget https://mirror.openshift.com/pub/openshift-v4/x86_64/clients/ocp/4.21.2/openshift-client-linux.tar.gz

wget https://mirror.openshift.com/pub/openshift-v4/x86_64/clients/ocp/4.21.2/openshift-install-linux.tar.gz
```

### Output

![Download OpenShift Client and Installer](images/03-download-openshift-binaries.png)

> **Figure 3.** Downloading the OpenShift Client (`oc`) and OpenShift Installer binaries from the official Red Hat mirror.

The OpenShift Client and Installer archives were successfully downloaded from the official Red Hat mirror. These binaries will be extracted and installed on the bastion host in the next step.

### Notes

- Download the client and installer only from the official Red Hat mirror.
- Ensure both binaries match the target OpenShift cluster version.
- Keep the downloaded archives in a temporary working directory before extraction.

---

### Extract the Archives

```bash
tar -xvf openshift-client-linux.tar.gz

tar -xvf openshift-install-linux.tar.gz
```

📷 **Screenshot**
![Extract OpenShift Binaries](images/04-extract-openshift-binaries.png)

> **Figure 4.** Extracting the OpenShift Client and Installer archives.

---

### Install the Binaries

```bash
sudo mv oc kubectl openshift-install /usr/local/bin/

sudo chmod +x \
/usr/local/bin/oc \
/usr/local/bin/kubectl \
/usr/local/bin/openshift-install
```

📷 **Screenshot**

![Install OpenShift Binaries](images/05-install-openshift-binaries.PNG)

> **Figure 5.** Installing the OpenShift Client, `kubectl`, and OpenShift Installer into `/usr/local/bin`.

---

### Verify Installation

Verify that both the OpenShift Client (`oc`) and OpenShift Installer (`openshift-install`) are installed successfully and that their versions match the target OpenShift release.

### Commands

```bash
oc version --client

openshift-install version
```

### Expected Output

- OpenShift Client Version: **4.21.2**
- OpenShift Installer Version: **4.21.2**

### Output

![Verify OpenShift Client and Installer](images/06-verify-openshift-version.PNG)

> **Figure 6.** Verification of the installed OpenShift Client (`oc`) and OpenShift Installer versions.

The output confirms that both the OpenShift Client and Installer have been installed successfully and are running the same release version. Using matching versions helps ensure compatibility throughout the cluster installation process.

### Notes

- Always use the same version of the OpenShift Client and Installer.
- Verify the binaries before generating installation assets.
- Store the binaries in `/usr/local/bin` so they are accessible system-wide.
---

### Verification Checklist

- ✅ Required packages installed
- ✅ Chrony service enabled
- ✅ Time synchronized
- ✅ OpenShift Client installed
- ✅ OpenShift Installer installed
- ✅ Correct OpenShift version verified


### Step 02: DNS Configuration

Configure the required DNS records in **Windows Active Directory DNS** before starting the OpenShift installation. Proper DNS resolution is essential for communication between the bootstrap node, control plane nodes, worker nodes, and the OpenShift API.

### Configure DNS Records

Create the following DNS records in the DNS Manager.

| Record | Type | Description |
|--------|------|-------------|
| api.ocp4.example.com | A | OpenShift API Virtual IP |
| api-int.ocp4.example.com | A | Internal OpenShift API Virtual IP |
| *.apps.ocp4.example.com | A | OpenShift Applications Virtual IP |
| bootstrap.ocp4.example.com | A | Bootstrap Node |
| master-0.ocp4.example.com | A | Control Plane Node 0 |
| master-1.ocp4.example.com | A | Control Plane Node 1 |
| master-2.ocp4.example.com | A | Control Plane Node 2 |
| worker-0.ocp4.example.com | A | Worker Node 0 |
| worker-1.ocp4.example.com | A | Worker Node 1 |

### Output

![Windows Active Directory DNS Records](images/07-dns-records.PNG)

> **Figure 7.** Windows Active Directory DNS records configured for the OpenShift cluster.

---

### Validate DNS Resolution

Verify that all DNS records resolve successfully from the bastion host.

### Commands

```bash
nslookup api.ocp4.example.com

nslookup api-int.ocp4.example.com

nslookup bootstrap.ocp4.example.com

nslookup master-0.ocp4.example.com

nslookup master-1.ocp4.example.com

nslookup master-2.ocp4.example.com

nslookup worker-0.ocp4.example.com

nslookup worker-1.ocp4.example.com
```

### Output

![DNS Validation from Bastion Host](images/08-dns-validation.png)

> **Figure 8.** Successful DNS resolution of the OpenShift API, bootstrap, control plane, and worker nodes from the bastion host.

The successful `nslookup` results confirm that all required DNS records are correctly configured and reachable from the bastion host. This verification must be completed before generating the OpenShift installation assets.

### Notes

- Verify all DNS records before starting the installation.
- Ensure both API and Applications Virtual IPs resolve correctly.
- Confirm that all cluster node hostnames resolve to their assigned static IP addresses.
- Any DNS resolution failure should be corrected before proceeding with the OpenShift installation.

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
