# OpenShift GitOps (Argo CD)

## Objective

This project demonstrates how to deploy and manage applications on OpenShift using GitOps with Argo CD.
                GitHub
                   │
                   │ git push
                   ▼
             Argo CD (GitOps)
                   │
             Sync Manifest
                   ▼
        OpenShift Cluster
                   │
                   ▼
      ConfigMap / Deployment / VM
