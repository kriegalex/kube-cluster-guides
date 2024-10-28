# Kubernetes Cluster Setup Documentation

Welcome to the **Kubernetes Cluster Setup Documentation** repository! This repository provides detailed, step-by-step guides to help you set up and manage various types of Kubernetes clusters, including `k8s`, `k3s`, and storage provisioning using Longhorn.

---

## Table of Contents

- [Introduction](#introduction)
- [Choosing the Right Setup Guide](#choosing-the-right-setup-guide)
- [Repository Structure](#repository-structure)
- [Getting Started](#getting-started)
- [Additional Resources](#additional-resources)

---

## Introduction

This repository is intended for system administrators, DevOps engineers, and developers who need to set up, manage, and troubleshoot Kubernetes clusters. I provide documentation for various Kubernetes distributions and configurations to accommodate different use cases, from local testing to cloud deployments.

## Choosing the Right Setup Guide

Use the table below to select the documentation best suited for your environment:

| Cluster Type       | Guide Location                                            | Description                                               |
|--------------------|-----------------------------------------------------------|-----------------------------------------------------------|
| **Simple K8s**     | [k8s/kubeadm-multi-node](guides/k8s/kubeadm-multi-node)   | Simple multi-node k8s without HA control plane            |
| **Lightweight K3s**| [k3s/single-node](guides/k3s/single-node)                 | Lightweight Kubernetes setup for testing, learning or for resource-limited devices |
| **Longhorn K3s**   | [k3s/ansible-longhorn](guides/k3s/ansible-longhorn)       | 3+ node setup with redundant storage using longhorn. Uses ansible for the configuration management. |

Refer to each guide’s **Prerequisites** to ensure you meet the requirements.

---

## Repository Structure

Here's an overview of the repository's organization to help you find relevant information easily:

```
kube-cluster-guides/ 
│
├── README.md # Main documentation overview 
├── guides/ # Main directory for setup guides 
│   ├── k8s/ # Standard Kubernetes setups 
│   │   ├── kubeadm-multi-node
│   │   └── ...
│   │ 
│   ├── k3s/ # Lightweight K3s Kubernetes setups 
│   │   ├── ansible-longhorn
│   │   └── ...
│   │ 
│   └── ...
│ 
└── docs/ # Additional reference documents 
    ├── glossary.md # Glossary of terms 
    ├── common-commands.md # Quick reference for Kubernetes commands 
    └── ...
```

### Key Files

- **[common-commands.md](docs/common-commands.md)**: A quick reference for frequently used Kubernetes and storage commands.
- **[glossary.md](docs/glossary.md)**: Terminology guide for Kubernetes concepts.

---

## Getting Started

1. **Review Prerequisites**: Each guide has a prerequisites section. Ensure your environment meets the requirements before starting.

2. **Select a Setup Guide**: Based on your needs (local testing, cloud deployment, lightweight setups), choose the appropriate guide in the [Choosing the Right Setup Guide](#choosing-the-right-setup-guide) section.

3. **Follow Setup Instructions**: Each guide provides clear, step-by-step instructions. Additionally, you can refer to the [common-commands.md](docs/common-commands.md) file for a quick command reference.

---

## Additional Resources

- **[Kubernetes Documentation](https://kubernetes.io/docs/)** - Official Kubernetes documentation.
- **[K3s Documentation](https://rancher.com/docs/k3s/latest/en/)** - Rancher's lightweight Kubernetes distribution documentation.
- **[Longhorn Documentation](https://longhorn.io/docs/)** - Documentation for Longhorn storage solutions.
