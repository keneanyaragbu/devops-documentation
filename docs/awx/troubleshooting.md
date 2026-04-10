# Troubleshooting Guide

## Issue 1 — ImagePullBackOff
**Symptom:**
Failed to pull image "gcr.io/kubebuilder/kube-rbac-proxy:v0.15.0": not found

**Cause:**
Google Container Registry (gcr.io) has been deprecated.

**Fix:**
```bash
kubectl edit deployment awx-operator-controller-manager -n awx
```
Replace:
gcr.io/kubebuilder/kube-rbac-proxy:v0.15.0
With:
quay.io/brancz/kube-rbac-proxy:v0.15.0

---

## Issue 2 — No Hosts Matched
**Symptom:**
Could not match supplied host pattern, ignoring: webservers
skipping: no hosts matched

**Cause:**
Hosts were added individually to inventory but not assigned to
a group called `webservers`.

**Fix:**
1. Go to Inventories → Worker-Nodes
2. Click Groups tab
3. Add group named `webservers`
4. Add both hosts to the group
5. Relaunch job template

---

## Issue 3 — AWX Shows Success But Nothing Changed
**Symptom:**
Job shows green Success but servers not updated.

**Cause:**
AWX reports success even when no hosts are matched.

**Fix:**
Always check job output for:

- `changed` → tasks actually ran ✅
- `ok` → already configured, no change needed
- `skipping` → hosts not matched ⚠️

---

## Issue 4 — Permission Denied on SSH Key
**Symptom:**
WARNING: UNPROTECTED PRIVATE KEY FILE!
Permissions 0644 for key.pem are too open.

**Fix:**
```bash
chmod 400 key.pem
```
