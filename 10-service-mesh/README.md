# Red Hat OpenShift Service Mesh

## Objective

This project demonstrates the deployment and validation of Red Hat OpenShift Service Mesh on OpenShift Container Platform.

Service Mesh provides secure, observable, and manageable communication between microservices using Istio, Kiali, Jaeger, and Envoy Proxy.

---

## Features

- Service Mesh Installation
- Istio Control Plane
- Traffic Management
- Service-to-Service Security (mTLS)
- Distributed Tracing
- Observability
- Service Topology

---

## Architecture

```text
              Client Request
                     │
                     ▼
               OpenShift Route
                     │
                     ▼
               Istio Ingress Gateway
                     │
        ┌────────────┴────────────┐
        ▼                         ▼
   Application A             Application B
        │                         │
        ▼                         ▼
     Envoy Proxy             Envoy Proxy
        │                         │
        └────────────┬────────────┘
                     ▼
                 Istiod Control Plane
```

---

## Lab Environment

| Component | Version |
|-----------|---------|
| OpenShift | 4.21.21 |
| Kubernetes | v1.34.x |
| Service Mesh | Red Hat OpenShift Service Mesh |
| Istio | Included |
| Platform | VMware vSphere ESXi |

---

## Components

- Istio
- Istiod
- Envoy Proxy
- Ingress Gateway
- Kiali
- Jaeger

---

## Verification Commands

```bash
oc get subscriptions -A

oc get csv -A

oc get pods -A | grep istio

oc get crd | grep istio

oc get namespace
```

---

## Learning Outcomes

- Service Mesh Architecture
- Istio
- Traffic Management
- mTLS
- Distributed Tracing
- Service Discovery

---

## Author

Ramandeep Singh

Senior Linux | OpenShift | Kubernetes Platform Engineer
