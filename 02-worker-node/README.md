# Worker Node Addition and CSR Troubleshooting

---

# Objective

The purpose of this document is to demonstrate the complete process of adding a new worker node into an existing OpenShift 4.21 UPI production cluster running on VMware ESXi.

This guide also includes CSR approval, troubleshooting and verification steps based on real-world deployment experience.

---

# Existing Cluster

| Component | Count |
|-----------|------|
| Masters | 3 |
| Workers | 5 |
| Platform | VMware ESXi |
| Installation | UPI |

---

# Task

Add a new Worker Node (worker-5) into the existing cluster.

---

# High Level Workflow

1. Deploy RHCOS Worker VM
2. Configure Network
3. Attach Worker Ignition
4. Boot Worker
5. Wait for CSR
6. Approve CSR
7. Verify Node Status
8. Verify MachineConfig
9. Validate Cluster Health

---

# CSR Verification

```bash
oc get csr
```

Approve Pending CSR

```bash
oc adm certificate approve <csr-name>
```

Approve All Pending CSRs

```bash
oc get csr --no-headers \
| awk '/Pending/ {print $1}' \
| xargs --no-run-if-empty oc adm certificate approve
```

---

# Node Verification

```bash
oc get nodes
```

Expected Output

- worker-5 Ready
- Scheduling Enabled

---

# Troubleshooting Performed

## Node NotReady

Verification performed

```bash
journalctl -b

journalctl -u kubelet

oc describe node worker-5

oc get csr

oc get mcp

oc get machineconfig
```

---

# Lessons Learned

- Verify Ignition before booting.
- Always approve pending CSRs.
- Check kubelet logs.
- Validate MachineConfigPool status.
- Verify Cluster Operators after node join.

---

# Final Result

✅ Worker Node Successfully Joined

✅ Node Ready

✅ Cluster Healthy
