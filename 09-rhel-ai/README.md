# Red Hat Enterprise Linux AI (RHEL AI)

## Objective

This project demonstrates the deployment and validation of Red Hat Enterprise Linux AI (RHEL AI) on an OpenShift AI platform.

RHEL AI provides an enterprise-ready platform for deploying, testing, and serving Large Language Models (LLMs) using optimized inference runtimes and Red Hat AI technologies.

---

## Features

- RHEL AI Integration
- Large Language Models (LLMs)
- Model Serving
- Enterprise AI Platform
- AI Inference
- Containerized AI Workloads

---

## Architecture

```text
               User Request
                     │
                     ▼
             OpenShift Route
                     │
                     ▼
            OpenShift AI Platform
                     │
                     ▼
            RHEL AI Runtime
                     │
                     ▼
              Large Language Model
                     │
                     ▼
               AI Response
```

---

## Lab Environment

| Component | Version |
|-----------|---------|
| OpenShift | 4.21.21 |
| Kubernetes | v1.34.x |
| OpenShift AI | Red Hat OpenShift AI |
| RHEL AI | Enterprise AI Runtime |
| Platform | VMware vSphere ESXi |

---

## Components

- RHEL AI Runtime
- Large Language Models
- AI Inference
- OpenShift AI
- Container Images
- Enterprise AI Stack

---

## Verification Commands

```bash
oc get pods -A | grep ai

oc get pods -n ai-lab

oc logs <pod-name> -n ai-lab

oc get inferenceservice -A
```

---

## Learning Outcomes

- Enterprise AI Platform
- RHEL AI
- Large Language Models
- AI Runtime
- Model Deployment
- OpenShift AI Integration

---

## Author

Ramandeep Singh

Senior Linux | OpenShift | Kubernetes Platform Engineer
