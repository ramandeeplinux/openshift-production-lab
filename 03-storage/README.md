# OpenShift Storage with TrueNAS CSI

## Objective

Integrate TrueNAS SCALE with Red Hat OpenShift using CSI drivers to provide dynamic NFS and iSCSI persistent storage for Kubernetes workloads.

---

# Environment

| Component | Value |
|-----------|-------|
| Platform | VMware ESXi |
| OpenShift Version | 4.21.x |
| Kubernetes Version | v1.34.x |
| Storage Platform | TrueNAS SCALE 25.x |
| CSI Driver | Democratic CSI |
| Protocols | NFS & iSCSI |
| Default StorageClass | truenas-nfs-csi |
| Additional StorageClass | truenas-iscsi-csi |

---

# Architecture

```
                OpenShift Cluster
                        │
                        │
              Persistent Volume Claim
                        │
                        │
                StorageClass (CSI)
                        │
        ┌───────────────┴────────────────┐
        │                                │
   NFS CSI Driver                 iSCSI CSI Driver
        │                                │
        └───────────────┬────────────────┘
                        │
                 TrueNAS SCALE
                        │
                 Storage Pool (ZFS)
```

---

# Features

- Dynamic PVC Provisioning
- CSI Driver Integration
- NFS ReadWriteMany (RWX)
- iSCSI ReadWriteOnce (RWO)
- Automatic Persistent Volume Creation
- Kubernetes Native Storage
- Production Ready Storage Platform

---

# Verification Commands

## Storage Classes

```bash
oc get storageclass
```

---

## Persistent Volume Claims

```bash
oc get pvc -A
```

---

## Persistent Volumes

```bash
oc get pv
```

---

## CSI Drivers

```bash
oc get csidriver
```

---

## CSI Controller Pods

```bash
oc get pods -n democratic-csi
```

---

# Screenshots

## 1. TrueNAS Dashboard

TrueNAS storage pool configured and healthy.

![TrueNAS Dashboard](images/truenas-dashboard.png)

---

## 2. Storage Classes

OpenShift StorageClasses successfully created.

```bash
oc get storageclass
```

![Storage Classes](images/storageclass.png)

---

## 3. Persistent Volume Claims

PVCs dynamically provisioned by TrueNAS CSI.

```bash
oc get pvc -A
```

![PVC Bound](images/pvc-bound.png)

---

## 4. Persistent Volumes

Persistent Volumes automatically created.

```bash
oc get pv
```

![Persistent Volumes](images/persistent-volumes.png)

---

## 5. CSI Driver

CSI Driver successfully registered.

```bash
oc get csidriver
```

![CSI Driver](images/csidriver.png)

---

## 6. CSI Controller

CSI Controller Pods running successfully.

```bash
oc get pods -n democratic-csi
```

![CSI Controller](images/csi-controller.png)

---

# Outcome

Successfully integrated TrueNAS SCALE with OpenShift using Democratic CSI.

### Achievements

- ✅ Dynamic Storage Provisioning
- ✅ NFS RWX Storage
- ✅ iSCSI RWO Storage
- ✅ CSI Driver Installed
- ✅ Automatic PV Creation
- ✅ PVC Successfully Bound
- ✅ Production Ready Storage Platform

---

# Technologies Used

- OpenShift
- Kubernetes
- TrueNAS SCALE
- Democratic CSI
- NFS
- iSCSI
- ZFS
- VMware ESXi

---

# Next Lab

➡️ **04-openshift-virtualization**

Deploy OpenShift Virtualization (KubeVirt), create Fedora virtual machines, configure live migration and integrate virtual workloads with the TrueNAS CSI storage platform.
