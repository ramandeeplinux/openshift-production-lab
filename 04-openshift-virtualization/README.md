# OpenShift Virtualization (KubeVirt)

## Overview

This document demonstrates the deployment and validation of **OpenShift Virtualization (KubeVirt)** on a production-style Red Hat OpenShift 4.21 cluster running on VMware vSphere ESXi.

The lab showcases enterprise virtualization capabilities including Linux virtual machines, dynamic storage provisioning, secondary networking, live migration and snapshot functionality.

---

# Environment

| Component | Version |
|-----------|---------|
| Red Hat OpenShift | 4.21.21 |
| Kubernetes | 1.34 |
| VMware vSphere ESXi | 8.x |
| OpenShift Virtualization | Latest Stable |
| Storage | TrueNAS CSI (NFS & iSCSI) |
| Networking | OVN Kubernetes + NMState |

---

# Architecture

```text
                   VMware ESXi
                        │
                        ▼
            Red Hat OpenShift 4.21
      ┌───────────────────────────────┐
      │        Control Plane          │
      │         Worker Nodes          │
      └───────────────────────────────┘
                        │
                        ▼
            OpenShift Virtualization
                 (KubeVirt)
                        │
         ┌──────────────┴──────────────┐
         │                             │
         ▼                             ▼
   Linux Virtual Machines         DataVolumes
         │                             │
         └──────────────┬──────────────┘
                        ▼
          TrueNAS CSI Storage Backend
             NFS (RWX) + iSCSI (RWO)
```

---

# Features

- OpenShift Virtualization Deployment
- KubeVirt Platform
- Fedora Virtual Machine
- Dynamic DataVolumes
- TrueNAS NFS CSI
- TrueNAS iSCSI CSI
- Live Migration
- Virtual Machine Snapshots
- Secondary Bridge Network
- Static IP Assignment
- SSH Access
- VNC Console
- Persistent Storage Integration

---

# Installation Verification

Verify Operator

```bash
oc get csv -n openshift-cnv
```

Verify Pods

```bash
oc get pods -n openshift-cnv
```

Verify KubeVirt

```bash
oc get kubevirt -n openshift-cnv
```

Expected

```
Available=True
Progressing=False
Degraded=False
```

---

# Storage Validation

Verify Storage Classes

```bash
oc get sc
```

Expected Storage Classes

- truenas-nfs-csi
- truenas-iscsi-csi

Verify PVC

```bash
oc get pvc -A
```

---

# Virtual Machine Deployment

Create Fedora Virtual Machine

OpenShift Console

```
Virtualization
↓

Virtual Machines
↓

Create Virtual Machine

↓

Fedora
```

Verify

```bash
oc get vm -A
```

Verify Running Instance

```bash
oc get vmi -A
```

---

# Secondary Network

Secondary bridge networking configured using **NMState**.

Verification

```bash
oc get nncp
```

Expected

```
STATUS = Available
SuccessfullyConfigured
```

---

# Live Migration

Verify Migration

```bash
oc get vmim -A
```

Migration completed successfully without VM downtime.

---

# Snapshot Validation

```bash
oc get volumesnapshot -A
```

Snapshots created successfully.

---

# Validation Checklist

- OpenShift Virtualization Installed
- KubeVirt Available
- Operator Running
- Fedora Virtual Machine Running
- Dynamic DataVolume Created
- TrueNAS NFS CSI Working
- TrueNAS iSCSI CSI Working
- Persistent Volume Claims Bound
- Live Migration Successful
- Snapshot Successful
- Secondary Network Configured
- Static IP Verified
- SSH Access Verified
- VM Console Accessible

---

# Screenshots

## 1. OpenShift Virtualization Operator

![OpenShift Virtualization Operator](images/01-operator-installed.png)

---

## 2. KubeVirt Deployment

![KubeVirt Deployment](images/02-kubevirt-deployed.png)

---

## 3. Storage Classes

![Storage Classes](images/03-storage-classes.png)

---

## 4. Fedora Virtual Machine

![Fedora Virtual Machine](images/04-fedora-vm.png)

---

## 5. Live Migration

![Live Migration](images/05-live-migration.png)

---

## 6. Secondary Network Configuration

![Secondary Network](images/06-secondary-network.png)

---

## 7. Running Virtual Machine

![Running Virtual Machine](images/07-vm-running.png)

---

## 8. Virtual Machine Console

![VM Console](images/08-vm-console.png)

---

## 9. Persistent Volume Claims

![Persistent Volume Claims](images/09-pvc-created.png)

---

## 10. Virtual Machine Snapshot

![Virtual Machine Snapshot](images/10-snapshot.png)

---

# Technologies Used

- Red Hat OpenShift
- Kubernetes
- OpenShift Virtualization
- KubeVirt
- VMware vSphere ESXi
- TrueNAS CSI
- NFS
- iSCSI
- OVN Kubernetes
- NMState
- Linux
- RHEL

---

# References

- https://docs.redhat.com/
- https://kubevirt.io/
- https://github.com/kubevirt/kubevirt
