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

oc get subscription -n openshift-gitops-operator

oc get csv -n openshift-gitops-operator

oc get pods -n openshift-gitops

OpenShift Console

↓

GitOps

↓

Open ArgoCD

↓

Login
