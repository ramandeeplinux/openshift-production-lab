# OpenShift Virtualization (KubeVirt)

## Objective

Deploy and configure OpenShift Virtualization (KubeVirt) on Red Hat OpenShift 4.21 to run Linux virtual machines with enterprise storage, live migration and external networking.

---

## Environment

| Component | Version |
|-----------|---------|
| OpenShift | 4.21.21 |
| Kubernetes | 1.34 |
| VMware ESXi | 8.x |
| OpenShift Virtualization | Latest Stable |
| Storage | TrueNAS CSI (NFS + iSCSI) |
| Network | OVN Kubernetes + Secondary Bridge |

---

## Architecture

```text
                 VMware ESXi
                      │
                      ▼
          OpenShift 4.21 Cluster
      ┌────────────────────────────┐
      │ Master Nodes (3)           │
      │ Worker Nodes (6)           │
      └────────────────────────────┘
                    │
                    ▼
        OpenShift Virtualization
             (KubeVirt)
                    │
      ┌─────────────┴─────────────┐
      │                           │
      ▼                           ▼
 Linux Virtual Machines     DataVolumes
      │                           │
      └─────────────┬─────────────┘
                    ▼
          TrueNAS CSI Storage
        (NFS RWX + iSCSI RWO)
```

---

## Features

- OpenShift Virtualization Installation
- Linux Virtual Machines
- DataVolumes
- TrueNAS NFS CSI Storage
- TrueNAS iSCSI CSI Storage
- Dynamic PVC Provisioning
- Live Migration
- VM Snapshots
- Secondary Network (Bridge)
- Static IP Assignment
- SSH Access
- VNC Console Access

---

## Installation

### Verify Operator

```bash
oc get csv -n openshift-cnv
```

```bash
oc get pods -n openshift-cnv
```

Expected

```
All operator pods Running
```

---

## Verify Installation

```bash
oc get kubevirt -n openshift-cnv
```

Expected

```
Phase: Deployed
```

---

## Storage Classes

```bash
oc get sc
```

Expected

```
truenas-nfs-csi
truenas-iscsi-csi
```

---

## Create Fedora Virtual Machine

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

---

## Verify Virtual Machine

```bash
oc get vm -A
```

```bash
oc get vmi -A
```

---

## External Network

Configured using

- NMState
- Linux Bridge
- Multus NetworkAttachmentDefinition

Virtual Machine receives dedicated LAN IP.

---

## Live Migration

Verify

```bash
oc get vmim -A
```

Migration completed successfully without VM downtime.

---

## Snapshots

```bash
oc get volumesnapshot -A
```

Snapshot creation completed successfully.

---

## Storage Validation

NFS Storage

- RWX
- Dynamic Provisioning
- VM Image Storage

iSCSI Storage

- RWO
- Block Storage
- High Performance

---

## Validation Checklist

- OpenShift Virtualization Installed
- KubeVirt Running
- Fedora VM Created
- VM Running Successfully
- DataVolume Created
- Dynamic PVC Provisioning
- NFS CSI Working
- iSCSI CSI Working
- Live Migration Successful
- Snapshot Successful
- Secondary Network Configured
- Static IP Working
- SSH Access Verified
- VNC Console Verified

---

## References

https://docs.redhat.com/

https://kubevirt.io/

https://github.com/kubevirt/kubevirt
