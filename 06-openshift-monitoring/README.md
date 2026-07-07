# OpenShift Monitoring

## Objective

This project demonstrates the monitoring capabilities available in Red Hat OpenShift.

The OpenShift Monitoring stack provides cluster-wide visibility into infrastructure, platform components, applications, and workloads using Prometheus, Alertmanager, and Thanos.

---

## Features

- Cluster Monitoring
- Prometheus Metrics Collection
- Alertmanager
- Thanos Query
- Node Exporter
- Metrics Server
- Cluster Utilization Dashboard

---

## Architecture

```text
                OpenShift Cluster
                       │
      ┌────────────────┴────────────────┐
      │                                 │
      ▼                                 ▼
 Prometheus                      Node Exporter
      │                                 │
      └──────────────┬──────────────────┘
                     │
                     ▼
                Alertmanager
                     │
                     ▼
                   Thanos
                     │
                     ▼
             OpenShift Console
```

---

## Components

| Component | Purpose |
|----------|---------|
| Prometheus | Metrics Collection |
| Alertmanager | Alert Processing |
| Thanos | Long-term Metrics Query |
| Node Exporter | Node Metrics |
| Metrics Server | Resource Metrics |

---

## Verification Commands

```bash
oc get co monitoring

oc get pods -n openshift-monitoring

oc get route -n openshift-monitoring

oc get servicemonitors -A

oc get prometheus -n openshift-monitoring

oc get alertmanager -n openshift-monitoring
```

---

## Learning Outcomes

- OpenShift Monitoring Architecture
- Prometheus
- Alertmanager
- Thanos
- Metrics Collection
- Cluster Health Monitoring

---

## Author

Ramandeep Singh

Senior Linux | OpenShift | Kubernetes Platform Engineer
