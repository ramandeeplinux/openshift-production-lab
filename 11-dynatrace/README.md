# Dynatrace Integration with OpenShift

## Objective

This project demonstrates the integration of Dynatrace with Red Hat OpenShift Container Platform for end-to-end observability, infrastructure monitoring, application performance monitoring (APM), log analytics, and AI-powered root cause analysis.

Dynatrace provides a unified observability platform that automatically discovers workloads, monitors Kubernetes clusters, and delivers actionable insights into platform and application health.

---

## Features

- Kubernetes Cluster Monitoring
- Infrastructure Monitoring
- Application Performance Monitoring (APM)
- Automatic Service Discovery
- Log Monitoring
- AI-Powered Root Cause Analysis
- Real-time Dashboards

---

## Architecture

```text
                  OpenShift Cluster
                         │
        ┌────────────────┴────────────────┐
        │                                 │
        ▼                                 ▼
   Dynatrace Operator              OneAgent DaemonSet
        │                                 │
        └──────────────┬──────────────────┘
                       ▼
               Dynatrace Platform
                       │
                       ▼
      Dashboards • Alerts • AI Engine
```

---

## Lab Environment

| Component | Version |
|-----------|---------|
| OpenShift | 4.21.21 |
| Kubernetes | v1.34.x |
| Platform | VMware vSphere ESXi |
| Monitoring | Dynatrace |
| Deployment | Operator |

---

## Components

- Dynatrace Operator
- OneAgent
- ActiveGate
- Kubernetes Monitoring
- Infrastructure Monitoring
- Application Monitoring

---

## Verification Commands

```bash
oc get subscriptions -A

oc get csv -A

oc get pods -A | grep dynatrace

oc get daemonset -A

oc get deployment -A
```

---

## Learning Outcomes

- OpenShift Observability
- Dynatrace Integration
- Infrastructure Monitoring
- Kubernetes Monitoring
- AI-powered Root Cause Analysis
- Application Performance Monitoring

---

## Author

Ramandeep Singh

Senior Linux | OpenShift | Kubernetes Platform Engineer
