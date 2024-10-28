# Kubernetes Multi-node Cluster Installation with Kubeadm

## Overview

This guide provides step-by-step instructions to set up a multi-node Kubernetes cluster using `kubeadm` on Ubuntu Server 22.04. It includes the setup of the control plane, worker nodes, container runtime, network addons, load balancer, ingress controller, and dynamic NFS provisioning.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Environment](#environment)
- [Control Plane Setup](docs/control-plane-setup.md)
- [Worker Node Setup](docs/worker-node-setup.md)
- [Network Addon Setup](docs/network-addon-setup.md)
- [Load Balancer Setup (MetalLB)](docs/metallb-setup.md)
- [Ingress Controller Setup (NGINX)](docs/ingress-setup.md)
- [NFS Dynamic Provisioning](docs/nfs-provisioner-setup.md)
- [Container Runtime Setup](docs/container-runtime-setup.md)
- [Miscellaneous](docs/misc.md)

## Prerequisites

- **Operating System**: Ubuntu 22.04.3 LTS with 6.5 kernel (HWE)
- **Hardware Requirements**: Fast local storage on each node
- **Network Requirements**: Simple home networking setup

## Environment

Ensure that all nodes (control plane and worker nodes) meet the following prerequisites:

- Ubuntu 22.04 installed
- User account with `sudo` privileges
- Access to the internet

## Optional Enhancements

- [ZSH Shell Setup](docs/zsh-setup.md)

## Setup

Follow instructions in the following order:

1. [Control Plane Setup](docs/control-plane-setup.md)
2. [Worker Node Setup](docs/worker-node-setup.md)
3. [Network Addon Setup](docs/network-addon-setup.md)
4. [Load Balancer Setup (MetalLB)](docs/metallb-setup.md)
5. [Ingress Controller Setup (NGINX)](docs/ingress-setup.md)
6. [(optional) NFS Dynamic Provisioning](docs/nfs-provisioner-setup.md)

Check [here](docs/misc.md) for miscellaneous information.