# KServe Model Serving

## Objective

This project demonstrates AI model serving on Red Hat OpenShift AI using KServe.

KServe enables scalable, serverless inference for machine learning models on Kubernetes. It simplifies deploying, managing, and scaling AI/ML models while integrating with OpenShift AI.

---

## Features

- KServe Installation
- Inference Services
- Serverless Model Serving
- Auto Scaling
- REST API Endpoints
- Model Lifecycle Management

---

## Architecture

```text
                 Client Request
                        │
                        ▼
                 OpenShift Route
                        │
                        ▼
                  KServe Gateway
                        │
                        ▼
                Inference Service
                        │
                        ▼
                 AI Model Runtime
                        │
                        ▼
                  Prediction Result
```

---

## Lab Environment

| Component | Version |
|-----------|---------|
| OpenShift | 4.21.21 |
| Kubernetes | v1.34.x |
| OpenShift AI | Red Hat OpenShift AI |
| Model Serving | KServe |

---

## Components

- KServe
- InferenceService
- Serving Runtime
- Model Runtime
- OpenShift Route

---

## Verification Commands

```bash
oc get inferenceservice -A

oc get servingruntime -A

oc get pods -A | grep kserve

oc get routes -A
```

---

## Learning Outcomes

- KServe Architecture
- AI Model Serving
- Inference Services
- Serverless Inference
- OpenShift AI Integration

---

## Author

Ramandeep Singh

Senior Linux | OpenShift | Kubernetes Platform Engineer
