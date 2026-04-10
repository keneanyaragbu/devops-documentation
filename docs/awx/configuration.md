# AWX Configuration Guide

## Overview
After installation, AWX needs to be configured with:

1. **Credentials** — SSH key to connect to worker nodes
2. **Inventory** — List of servers to manage
3. **Project** — Link to GitHub playbook repository
4. **Job Template** — Combine all above and run!

## Step 1 — Add Credentials
Credentials tell AWX how to SSH into your worker nodes.

1. Click **Credentials** on left menu
2. Click **Add**
3. Fill in:

| Field | Value |
|---|---|
| Name | Worker-SSH-Key |
| Organization | Default |
| Credential Type | Machine |
| SSH Private Key | Paste contents of key.pem |

4. Click **Save**

!!! tip
    Get your key contents with: cat key.pem
    Copy everything including BEGIN and END lines.

## Step 2 — Create Inventory
Inventory tells AWX which servers to manage.

1. Click **Inventories** on left menu
2. Click **Add** then **Add Inventory**
3. Fill in:

| Field | Value |
|---|---|
| Name | Worker-Nodes |
| Organization | Default |

4. Click **Save**

### Add Hosts
5. Click on **Worker-Nodes** inventory
6. Click **Hosts** tab
7. Click **Add** for each worker:

**Server 1:**
```yaml
Name: Server1
Variables:
  ansible_host: worker-1-ip
  ansible_user: ec2-user
```

**Server 2:**
```yaml
Name: Server2
Variables:
  ansible_host: worker-2-ip
  ansible_user: ec2-user
```

### Add Group

!!! warning
    This step is critical! Without a group your playbook
    will not match any hosts and nothing will run!

8. Click **Groups** tab
9. Click **Add**
10. Name it exactly: webservers
11. Click **Save**
12. Click on **webservers** group
13. Click **Hosts** tab
14. Click **Add existing host**
15. Add both Server1 and Server2

## Step 3 — Create Project
Project links AWX to your GitHub repository.

1. Click **Projects** on left menu
2. Click **Add**
3. Fill in:

| Field | Value |
|---|---|
| Name | Ansible-Webserver-Project |
| Organization | Default |
| Source Control Type | Git |
| Source Control URL | https://github.com/keneanyaragbu/ansible-webservers |
| Branch | main |

4. Check **Update Revision on Launch**
5. Click **Save**

!!! success
    A green circle confirms successful sync with GitHub

## Step 4 — Create Job Template
Job Template combines everything into one runnable job.

1. Click **Templates** on left menu
2. Click **Add** then **Add Job Template**
3. Fill in:

| Field | Value |
|---|---|
| Name | Deploy-Webserver |
| Job Type | Run |
| Inventory | Worker-Nodes |
| Project | Ansible-Webserver-Project |
| Playbook | webserver.yml |
| Credentials | Worker-SSH-Key |

4. Click **Save**
5. Click **Launch**

## Step 5 — Verify Deployment
After job completes check:

- Job status shows **Successful**
- Tasks show **changed** not **skipping**
- Open browser at worker node IP
- Webpage shows updated content

## Workflow Summary
```
Edit code on GitHub
        ↓
AWX pulls latest code automatically
        ↓
Ansible playbook executes across servers
        ↓
Both servers updated simultaneously
        ↓
Live webpage updated!
```
