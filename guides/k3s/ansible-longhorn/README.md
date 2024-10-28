# Setting Up a 3-Node k3s Kubernetes Cluster on Ubuntu Server with Longhorn, Helm, and Ansible

## Introduction

This repository contains a comprehensive, step-by-step guide to setting up a highly available 3-node **k3s Kubernetes cluster** on **Ubuntu Server 24.04 LTS** (or **22.04 LTS** if 24.04 is not available). The tutorial incorporates **Longhorn** for persistent storage, **Helm** for package management, and **Ansible** for automation and configuration management. To test the cluster, we'll deploy **Plex** and **Jellyfin** media servers.

A [Glossary](../../../docs/glossary.md) explaining Kubernetes and DevOps concepts used throughout is also available.

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Setting Up Ubuntu Servers](#setting-up-ubuntu-servers)
3. [Configuring SSH Access](#configuring-ssh-access)
4. [Installing Ansible](#installing-ansible)
5. [Preparing Ansible Inventory and Playbooks](#preparing-ansible-inventory-and-playbooks)
6. [Deploying k3s Cluster with Ansible](#deploying-k3s-cluster-with-ansible)
7. [Deploying Plex and Jellyfin with Ansible](#deploying-plex-and-jellyfin-with-ansible)
8. [Testing the Deployment](#testing-the-deployment)
9. [Conclusion](#conclusion)
10. [Additional Information](#additional-information)

---

## Prerequisites

- **Three Ubuntu Server machines** running **Ubuntu 24.04 LTS** or **22.04 LTS**.
- **Ansible Control Node**: Can be one of the Ubuntu servers or a separate machine.
- **SSH Access**: Root or a user with sudo privileges.
- **Stable Internet Connection** for downloading packages and container images.

---

## Setting Up Ubuntu Servers

### 1. Update and Upgrade the System

Login to **each Ubuntu server** and run:

```bash
sudo apt update && sudo apt upgrade -y
```

### 2. Configure Hostnames and Hosts File

Assign unique hostnames to each server.

On the **master node**:

```bash
sudo hostnamectl set-hostname k3s-master
```

On the **worker nodes**:

```bash
sudo hostnamectl set-hostname k3s-worker1
sudo hostnamectl set-hostname k3s-worker2
```

Update `/etc/hosts` on **all nodes**:

```bash
sudo nano /etc/hosts
```

Add the following lines (replace with your servers' IP addresses):

```
192.168.1.100 k3s-master
192.168.1.101 k3s-worker1
192.168.1.102 k3s-worker2
```

---

## Configuring SSH Access

### 1. Generate SSH Keys on Ansible Control Node

If you don't have an SSH key pair, generate one:

```bash
ssh-keygen -t rsa -b 4096
```

### 2. Copy SSH Keys to All Nodes

Replace `user` with your actual username:

```bash
ssh-copy-id user@k3s-master
ssh-copy-id user@k3s-worker1
ssh-copy-id user@k3s-worker2
```

---

## Installing Ansible

### 1. Install Ansible on Control Node

On your **Ansible control node**, run:

```bash
sudo apt update
sudo apt install -y ansible
```

### 2. Verify Ansible Installation

```bash
ansible --version
```

---

## Preparing Ansible Inventory and Playbooks

### 1. Create Ansible Project Directory

Create a directory for your Ansible project:

```bash
mkdir ~/k3s-cluster && cd ~/k3s-cluster
```

### 2. Create Ansible Inventory File

Create an `inventory.ini` file:

```ini
[master]
k3s-master ansible_user=user

[workers]
k3s-worker1 ansible_user=user
k3s-worker2 ansible_user=user

[all:vars]
ansible_python_interpreter=/usr/bin/python3
```

**Note**: Replace `user` with your actual username on the servers.

### 3. Create Ansible Playbook for Cluster Setup

Create a `site.yml` playbook:

<details>
<summary><strong>Click to view <code>site.yml</code></strong></summary>

```yaml
---
- name: Install Dependencies and Set Up Cluster
  hosts: all
  become: yes
  tasks:
    - name: Install required packages
      apt:
        name:
          - curl
          - apt-transport-https
          - ca-certificates
        state: present
        update_cache: yes

- name: Install k3s Master Node
  hosts: master
  become: yes
  tasks:
    - name: Install k3s master
      shell: |
        curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="--disable traefik" sh -
      args:
        executable: /bin/bash

    - name: Retrieve k3s token
      slurp:
        src: /var/lib/rancher/k3s/server/node-token
      register: k3s_token

    - name: Save k3s token to a file
      copy:
        content: "{{ k3s_token.content | b64decode }}"
        dest: ~/k3s_token

    - name: Copy kubeconfig to user directory
      copy:
        src: /etc/rancher/k3s/k3s.yaml
        dest: /home/{{ ansible_user }}/.kube/config
        remote_src: yes
        owner: "{{ ansible_user }}"
        group: "{{ ansible_user }}"
        mode: 0600

    - name: Update kubeconfig server address
      replace:
        path: /home/{{ ansible_user }}/.kube/config
        regexp: '127.0.0.1'
        replace: '{{ inventory_hostname }}'

- name: Install k3s Worker Nodes
  hosts: workers
  become: yes
  tasks:
    - name: Fetch k3s token from master
      fetch:
        src: ~/k3s_token
        dest: ./k3s_token
        flat: yes

    - name: Join k3s worker node
      shell: |
        TOKEN=$(cat ./k3s_token)
        curl -sfL https://get.k3s.io | K3S_URL=https://k3s-master:6443 K3S_TOKEN=$TOKEN sh -
      args:
        executable: /bin/bash

- name: Install Helm on Master Node
  hosts: master
  become: yes
  tasks:
    - name: Download Helm install script
      get_url:
        url: https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
        dest: /tmp/get-helm-3
        mode: '0755'

    - name: Install Helm
      shell: /tmp/get-helm-3
      args:
        executable: /bin/bash

- name: Install Longhorn via Helm
  hosts: master
  become: yes
  tasks:
    - name: Create Longhorn namespace
      kubernetes.core.k8s:
        state: present
        definition:
          kind: Namespace
          apiVersion: v1
          metadata:
            name: longhorn-system

    - name: Add Longhorn Helm repository
      shell: |
        helm repo add longhorn https://charts.longhorn.io
        helm repo update
      args:
        executable: /bin/bash

    - name: Install Longhorn using Helm
      helm:
        name: longhorn
        chart_repo: https://charts.longhorn.io
        chart: longhorn
        namespace: longhorn-system
        create_namespace: false
        wait: true
```

</details>

### 4. Create Ansible Playbook for Application Deployment

Create a `deploy-apps.yml` playbook:

<details>
<summary><strong>Click to view <code>deploy-apps.yml</code></strong></summary>

```yaml
---
- name: Deploy Plex and Jellyfin
  hosts: master
  become: yes
  tasks:
    - name: Add k8s-at-home Helm repository
      shell: |
        helm repo add k8s-at-home https://k8s-at-home.com/charts/
        helm repo update
      args:
        executable: /bin/bash

    - name: Create namespace for media servers
      kubernetes.core.k8s:
        state: present
        definition:
          kind: Namespace
          apiVersion: v1
          metadata:
            name: media-servers

    - name: Install Plex via Helm
      helm:
        name: plex
        chart: k8s-at-home/plex
        namespace: media-servers
        values:
          persistence:
            config:
              enabled: true
              storageClass: longhorn
            data:
              enabled: true
              storageClass: longhorn
        wait: true

    - name: Install Jellyfin via Helm
      helm:
        name: jellyfin
        chart: k8s-at-home/jellyfin
        namespace: media-servers
        values:
          persistence:
            config:
              enabled: true
              storageClass: longhorn
            cache:
              enabled: true
              storageClass: longhorn
        wait: true
```

</details>

---

## Deploying k3s Cluster with Ansible

### 1. Run the Ansible Playbook

Ensure you're in the `~/k3s-cluster` directory and run:

```bash
ansible-playbook -i inventory.ini site.yml
```

### 2. Verify the Cluster

On the **master node** or **Ansible control node**:

```bash
kubectl get nodes
```

You should see all three nodes in the **Ready** state.

---

## Deploying Plex and Jellyfin with Ansible

### 1. Run the Application Deployment Playbook

```bash
ansible-playbook -i inventory.ini deploy-apps.yml
```

### 2. Verify Deployments

```bash
kubectl -n media-servers get pods
```

All pods should be in the **Running** state.

---

## Testing the Deployment

### 1. Access Plex and Jellyfin

Retrieve the service information:

```bash
kubectl -n media-servers get svc
```

You should see services for **plex** and **jellyfin**.

### 2. Port Forwarding (If Necessary)

If you don't have LoadBalancer or Ingress set up, you can use port forwarding:

```bash
kubectl -n media-servers port-forward svc/plex 32400:32400
kubectl -n media-servers port-forward svc/jellyfin 8096:8096
```

Access Plex at **[http://localhost:32400/web](http://localhost:32400/web)** and Jellyfin at **[http://localhost:8096](http://localhost:8096)**.

---

## Conclusion

By automating the installation and configuration of **k3s**, **Helm**, **Longhorn**, and your applications using **Ansible**, you've set up a reproducible and manageable Kubernetes environment. This approach aligns with modern best practices, ensuring that your infrastructure is consistent and easy to maintain.

---

## Additional Information

- **Ansible Modules Used**:
  - `apt`: For package management on Ubuntu.
  - `shell`: To execute shell commands where necessary.
  - `slurp` and `fetch`: To handle file content across nodes.
  - `copy`: For copying files and content.
  - `kubernetes.core.k8s`: To interact with Kubernetes resources.
  - `helm`: An Ansible module to manage Helm charts.

- **Best Practices Implemented**:
  - **Infrastructure as Code**: All configurations are codified in Ansible playbooks.
  - **Idempotency**: Ansible ensures tasks have the same result regardless of how many times they are executed.
  - **Version Control**: Store your Ansible playbooks in a version control system like Git.

---

## Glossary

Please refer to the [Glossary](../../../docs/glossary.md) for explanations of Kubernetes and DevOps concepts used in this tutorial.

---

## License

This project is licensed under the terms of the MIT license. See the [LICENSE](LICENSE) file for details.

---

## Contributing

Contributions are welcome! Please open an issue or submit a pull request for any improvements or additions.

