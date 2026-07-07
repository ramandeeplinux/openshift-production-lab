# OpenShift GitOps (Argo CD)

## Objective

This project demonstrates how to deploy and manage Kubernetes resources on Red Hat OpenShift using OpenShift GitOps (Argo CD).

OpenShift GitOps follows the GitOps methodology where the desired state of the cluster is stored in a Git repository. Argo CD continuously monitors the repository and automatically synchronizes changes to the OpenShift cluster.

---

## Features

- OpenShift GitOps (Argo CD) deployment
- GitHub repository integration
- Continuous Deployment (CD)
- Infrastructure as Code (IaC)
- Automated application synchronization
- Kubernetes YAML manifest management
- Declarative application deployment

---

## Architecture

```text
                Git Push
                    │
                    ▼
          GitHub Repository
                    │
                    ▼
         OpenShift GitOps (Argo CD)
                    │
                    ▼
      Kubernetes YAML Manifests
                    │
                    ▼
          OpenShift Cluster
```

---

## Lab Environment

| Component | Version |
|-----------|---------|
| OpenShift | 4.21.21 |
| Platform | VMware vSphere ESXi |
| Kubernetes | v1.34.x |
| GitOps | OpenShift GitOps (Argo CD) |
| Git Repository | GitHub |
| CLI | OpenShift CLI (oc) |

---

## Repository Structure

```
05-openshift-gitops/
├── README.md
├── images/
│   ├── gitops-dashboard.png
│   ├── argocd-route.png
│   ├── application-sync.png
│   └── gitops-topology.png
└── manifests/
    ├── namespace.yaml
    ├── application.yaml
    ├── deployment.yaml
    └── configmap.yaml
```

---

## Workflow

1. Developer updates Kubernetes YAML manifests.
2. Changes are committed and pushed to GitHub.
3. Argo CD detects repository changes.
4. Application state is synchronized automatically.
5. OpenShift cluster reaches the desired state.

---

## Sample Commands

### Verify Operator

```bash
oc get subscriptions -A
oc get csv -A
```

### Verify Argo CD

```bash
oc get argocd -A
oc get pods -n openshift-gitops
oc get route -n openshift-gitops
```

### Verify Application

```bash
oc get applications -n openshift-gitops
```

### Verify Synchronization

```bash
oc get all -n virtualization
```

---

## Screenshots

Add screenshots inside the **images** directory.

Example:

- OpenShift Console
- GitOps Dashboard
- Argo CD Route
- Application Health
- Synced Status
- GitHub Repository
- OpenShift Resources

---

## Learning Outcomes

This project demonstrates practical experience with:

- OpenShift GitOps
- Argo CD
- GitHub Integration
- Continuous Deployment
- Infrastructure as Code
- Kubernetes Manifests
- OpenShift Administration

---

## Author

**Ramandeep Singh**

Senior Linux | OpenShift | Kubernetes Platform Engineer

GitHub

https://github.com/ramandeeplinux

---

## Next Documentation

- OpenShift AI
- KServe
- RHEL AI
- Grafana
- Prometheus
- OpenShift Virtualization
