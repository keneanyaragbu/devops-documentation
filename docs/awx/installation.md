# AWX Installation Guide

## Prerequisites
| Resource | Requirement |
|---|---|
| OS | Ubuntu 22.04 |
| Instance Type | t2.large |
| RAM | 8GB minimum |
| Disk | 20GB minimum |
| CPUs | 2 minimum |

## Security Group Configuration
| Port | Protocol | Source | Reason |
|---|---|---|---|
| 22 | TCP | Your IP only | SSH admin access |
| 80 | TCP | 0.0.0.0/0 | AWX Web UI |
| 443 | TCP | 0.0.0.0/0 | AWX Web UI HTTPS |
| 30701 | TCP | Your IP | AWX NodePort |

## Step 1 — Install Dependencies
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y git curl wget python3 python3-pip
```

## Step 2 — Install Docker
```bash
sudo apt install -y docker.io
sudo systemctl start docker
sudo systemctl enable docker
```

## Step 3 — Install K3s
```bash
curl -sfL https://get.k3s.io | sh -
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
echo "export KUBECONFIG=/etc/rancher/k3s/k3s.yaml" >> ~/.bashrc
```

Verify K3s is running:
```bash
kubectl get nodes
```

Expected output:
NAME                STATUS   ROLES           AGE   VERSION
ip-172-xx-xx-xx    Ready    control-plane   98s   v1.34.6+k3s1

## Step 4 — Create AWX Namespace
```bash
kubectl create namespace awx
```

## Step 5 — Deploy AWX Operator
```bash
kubectl apply -f https://raw.githubusercontent.com/ansible/awx-operator/devel/deploy/awx-operator.yaml -n awx
```

## Step 6 — Fix Image Registry Issue
!!! warning
    gcr.io has been deprecated. You must replace the image with quay.io
```bash
kubectl edit deployment awx-operator-controller-manager -n awx
```

Change:
gcr.io/kubebuilder/kube-rbac-proxy:v0.15.0

To:
quay.io/brancz/kube-rbac-proxy:v0.15.0

Save with **Esc** then **:wq**

## Step 7 — Verify Operator is Running
```bash
kubectl get pods -n awx
```

Expected:
NAME                                              READY   STATUS
awx-operator-controller-manager-ccbc476dc-gvgjz   2/2     Running

## Step 8 — Deploy AWX Instance
```bash
kubectl apply -f awx-instance.yml
```

Contents of awx-instance.yml:
```yaml
apiVersion: awx.ansible.com/v1beta1
kind: AWX
metadata:
  name: awx
  namespace: awx
spec:
  service_type: nodeport
```

## Step 9 — Watch Pods Come Up
```bash
kubectl get pods -n awx --watch
```

Wait until all pods show Running:
awx-migration        Completed
awx-operator         2/2 Running
awx-postgres-15      1/1 Running
awx-task             4/4 Running
awx-web              3/3 Running

## Step 10 — Get NodePort
```bash
kubectl get svc -n awx
```

Look for:
awx-service    NodePort    10.43.x.x    <none>    80:30701/TCP

## Step 11 — Get Admin Password
```bash
kubectl get secret awx-admin-password -n awx \
-o jsonpath="{.data.password}" | base64 --decode && echo ""
```

## Step 12 — Access AWX Web UI
http://<EC2-Public-IP>:30701
Username: admin
Password: (from Step 11)


