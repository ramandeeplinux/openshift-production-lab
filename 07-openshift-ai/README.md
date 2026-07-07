# OpenShift AI

## Objective

This project demonstrates the deployment and validation of Red Hat OpenShift AI on an OpenShift Container Platform cluster.

The environment provides a platform for building, deploying, and managing AI/ML workloads using Jupyter Notebooks, Model Serving, Data Science Projects, and integrated AI services.

---

## Features

- OpenShift AI Operator
- Jupyter Notebooks
- Data Science Projects
- Workbenches
- Model Serving
- AI Applications
- Integrated Monitoring

---

## Architecture

```text
                OpenShift Cluster
                        │
        ┌───────────────┴───────────────┐
        │                               │
        ▼                               ▼
 OpenShift AI Operator           Data Science Projects
        │                               │
        ▼                               ▼
  Workbenches                    Jupyter Notebook
        │
        ▼
  Model Serving
        │
        ▼
   AI Applications
```

---

## Lab Environment

| Component | Version |
|-----------|---------|
| OpenShift | 4.21.21 |
| Platform | VMware ESXi |
| OpenShift AI | Red Hat OpenShift AI |
| Kubernetes | v1.34.x |

---

## Components

- OpenShift AI Operator
- Data Science Projects
- Workbenches
- Jupyter Notebook
- Model Serving
- AI Dashboard

---

## Verification Commands

```bash
oc get pods -A | grep ods

oc get pods -n redhat-ods-applications

oc get notebook -A

oc get servingruntime -A

oc get inferenceService -A
```

---

## Learning Outcomes

- OpenShift AI Platform
- Data Science Workbench
- AI Model Deployment
- Jupyter Notebook
- Model Serving
- AI Application Management

---

## Author

Ramandeep Singh

Senior Linux | OpenShift | Kubernetes Platform Engineer
