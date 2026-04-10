# AWX Ansible Tower Project — Overview

## What is AWX?
AWX is the open source version of Red Hat Ansible Tower. It provides
a web-based user interface, REST API, and task engine built on top
of Ansible. It is the enterprise way of managing Ansible at scale.

## Project Goal
Deploy AWX on a lightweight Kubernetes cluster (K3s) on AWS EC2
and use it to orchestrate Ansible playbook execution across
multiple managed worker nodes.

## Architecture
AWX Web UI (Browser)
↓
AWX Server (Ubuntu EC2 - t2.large)
├── K3s (Lightweight Kubernetes)
│     └── AWX Operator
│           ├── awx-web     (Web UI)
│           ├── awx-task    (Job Execution)
│           └── awx-postgres (Database)
↓
Worker Nodes (Amazon Linux EC2)
├── Server 1 (54.157.103.132)
└── Server 2 (100.54.72.121)

## Technologies Used
| Technology | Purpose |
|---|---|
| AWX | Enterprise Ansible automation UI |
| K3s | Lightweight Kubernetes cluster |
| kubectl | Kubernetes CLI tool |
| kustomize | Kubernetes configuration management |
| AWS EC2 | Cloud compute instances |
| Ubuntu | AWX server OS |
| Amazon Linux | Worker node OS |
| GitHub | Playbook source control |

## Key Concepts to Learn
- Kubernetes pods, nodes and namespaces
- AWX Operator deployment and management
- Enterprise Ansible automation via Web UI
- GitHub integration with AWX
- SSH credential management
- Inventory and group management in AWX
- Idempotency in Ansible
- Image registry migration (gcr.io → quay.io)

## Project Outcome
Successfully deployed AWX on Kubernetes and used it to
automatically configure and deploy Nginx web servers across
multiple AWS EC2 worker nodes via a professional web-based
automation platform — with zero manual SSH intervention.
