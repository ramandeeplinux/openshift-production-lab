# 🚀 Red Hat OpenShift Enterprise Platform Engineering Lab

![OpenShift](https://img.shields.io/badge/OpenShift-4.21-red?style=for-the-badge&logo=redhat)
![Kubernetes](https://img.shields.io/badge/Kubernetes-v1.34-blue?style=for-the-badge&logo=kubernetes)
![VMware](https://img.shields.io/badge/VMware-vSphere-607078?style=for-the-badge&logo=vmware)
![GitOps](https://img.shields.io/badge/GitOps-ArgoCD-orange?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)
![Platform](https://img.shields.io/badge/Platform-Engineering-success?style=for-the-badge)

---

## 📖 Overview

This repository showcases a **production-style Red Hat OpenShift Platform Engineering Lab** built from scratch on **VMware vSphere ESXi**.

The primary objective is to demonstrate hands-on experience in designing, deploying, operating, and troubleshooting enterprise-grade OpenShift environments using modern Platform Engineering practices.

The lab covers the complete lifecycle of an OpenShift platform, including installation, worker node expansion, enterprise storage integration, virtualization, GitOps, monitoring, AI/ML workloads, service mesh, observability, and Day-2 platform operations.

---

# 🏗 Enterprise Lab Architecture

```text
                         VMware vSphere ESXi
                                 │
        ┌────────────────────────┴────────────────────────┐
        │                                                 │
        ▼                                                 ▼
   OpenShift Control Plane                        Worker Nodes
        │                                                 │
        ├──────── Kubernetes Cluster ─────────────────────┤
        │
        ├── OpenShift Virtualization (KubeVirt)
        ├── TrueNAS CSI (NFS)
        ├── TrueNAS CSI (iSCSI)
        ├── GitOps (Argo CD)
        ├── Prometheus
        ├── Alertmanager
        ├── OpenShift Monitoring
        ├── OpenShift AI
        ├── KServe
        ├── RHEL AI
        ├── Service Mesh (Istio)
        └── Dynatrace
```

---

# 🛠 Technology Stack

| Category | Technology |
|-----------|------------|
| Platform | Red Hat OpenShift 4.21 |
| Container Orchestration | Kubernetes |
| Operating System | Red Hat Enterprise Linux |
| Hypervisor | VMware vSphere ESXi |
| Virtualization | OpenShift Virtualization (KubeVirt) |
| Storage | TrueNAS CSI (NFS & iSCSI) |
| GitOps | Argo CD |
| Monitoring | Prometheus, Alertmanager |
| AI Platform | OpenShift AI |
| Model Serving | KServe |
| AI Runtime | RHEL AI |
| Service Mesh | Istio |
| Observability | Dynatrace |

---

# 📂 Repository Structure

| Folder | Description |
|---------|-------------|
| 📁 01-cluster-install | OpenShift Cluster Installation |
| 📁 02-worker-node | Worker Node Expansion |
| 📁 03-storage | TrueNAS CSI (NFS & iSCSI) |
| 📁 04-openshift-virtualization | KubeVirt & Live Migration |
| 📁 05-openshift-gitops | GitOps using Argo CD |
| 📁 06-openshift-monitoring | Prometheus & Alertmanager |
| 📁 07-openshift-ai | OpenShift AI |
| 📁 08-kserve | AI Model Serving |
| 📁 09-rhel-ai | Enterprise AI Runtime |
| 📁 10-service-mesh | Istio Service Mesh |
| 📁 11-dynatrace | Enterprise Observability |
| 📁 12-day2-operations | Production Day-2 Operations |

---

# 🖥 Lab Environment

| Component | Details |
|-----------|---------|
| OpenShift Version | 4.21.21 |
| Kubernetes | v1.34.x |
| Infrastructure | VMware vSphere ESXi |
| Storage | TrueNAS CSI (NFS & iSCSI) |
| Virtualization | OpenShift Virtualization |
| GitOps | Argo CD |
| Monitoring | Prometheus & Alertmanager |
| AI Platform | OpenShift AI |
| Model Serving | KServe |
| Service Mesh | Istio |

---

# ✅ Skills Demonstrated

- Red Hat OpenShift Administration
- Kubernetes Administration
- Platform Engineering
- VMware vSphere Administration
- OpenShift Virtualization
- Enterprise Storage Administration
- TrueNAS CSI Integration
- GitOps using Argo CD
- Platform Monitoring
- AI Platform Deployment
- Service Mesh
- Enterprise Troubleshooting
- Production Day-2 Operations

---

# 📸 Screenshots

The following documentation is available throughout this repository.

- OpenShift Web Console
- Cluster Installation
- Cluster Operators
- Worker Nodes
- Storage Classes
- Persistent Volumes
- OpenShift Virtualization
- Live Migration
- Argo CD
- Prometheus
- Alertmanager
- OpenShift AI
- KServe
- Dynatrace

> Screenshots are available inside the respective project folders.

---

# 📚 Learning Objectives

This repository demonstrates practical implementation of:

- Production-style OpenShift deployment
- Kubernetes platform administration
- Enterprise storage integration
- GitOps workflows
- Virtualization using KubeVirt
- Monitoring & Alerting
- AI/ML workloads on OpenShift
- Service Mesh
- Day-2 Operations
- Enterprise troubleshooting

---

# 🎯 Future Roadmap

Planned future enhancements include:

- OpenShift Pipelines (Tekton)
- Red Hat Advanced Cluster Management (ACM)
- Red Hat Quay Registry
- OpenShift Data Foundation (ODF)
- Multi-Cluster Management
- Disaster Recovery
- Bare Metal OpenShift Deployment
- CI/CD Pipelines
- Security Hardening
- Compliance Automation

---

# 👨‍💻 About Me

## Ramandeep Singh

**Senior Linux | Red Hat OpenShift | Kubernetes Platform Engineer**

20+ years of experience in Enterprise Linux Administration, VMware Virtualization, OpenShift Platform Engineering, Kubernetes, Enterprise Storage, GitOps, Monitoring and Cloud Native Technologies.

### Certifications

- Red Hat Certified Engineer (RHCE)
- Red Hat Certified Specialist in OpenShift Administration (EX280)
- Certified Kubernetes Administrator (CKA)
- Certified Kubernetes Security Specialist (CKS)
- Microsoft Azure AI Fundamentals
- Dell EMC Associate

---

# 📬 Connect with Me

- **GitHub:** https://github.com/ramandeeplinux
- **LinkedIn:** https://linkedin.com/in/ramandeep-singh-a0884718
- **Email:** ramandeep.linux@gmail.com

---

# ⭐ Support

If you find this repository useful, consider giving it a ⭐ to support the project and follow future OpenShift Platform Engineering updates.

---

## 📄 License

This project is licensed under the **MIT License**.
