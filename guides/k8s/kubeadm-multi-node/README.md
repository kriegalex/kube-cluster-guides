# Kubernetes Multi-node Cluster Installation with Kubeadm

## Overview

This guide provides step-by-step instructions to set up a multi-node Kubernetes cluster using `kubeadm` on Ubuntu Server 22.04. It includes the setup of the control plane, worker nodes, container runtime, network addons, load balancer, ingress controller, and dynamic NFS provisioning.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Optional Enhancements](#optional-enhancements)
- [Control Plane Setup](docs/control-plane-setup.md)
- [Worker Node Setup](docs/worker-node-setup.md)
- [Network Addon Setup](docs/network-addon-setup.md)
- [Load Balancer Setup (MetalLB)](docs/metallb-setup.md)
- [Ingress Controller Setup (NGINX)](docs/ingress-setup.md)
- [NFS Dynamic Provisioning](docs/nfs-provisioner-setup.md)
- [Container Runtime Setup](docs/container-runtime-setup.md)
- [Miscellaneous](docs/misc.md)

## Prerequisites

- **Two or more Ubuntu Server machines** running **22.04 LTS** (might also work on Ubuntu 24.04 LTS).
- **SSH Access**: Root or a user with sudo privileges.
- **Stable Internet Connection** for downloading packages and container images.

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