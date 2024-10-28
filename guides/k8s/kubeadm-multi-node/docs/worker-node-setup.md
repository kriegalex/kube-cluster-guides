# Worker Node Setup

Follow these steps on each worker node to join them to the cluster.

## Prerequisites

Ensure that the worker nodes meet the same prerequisites as the control plane node:

- Disable swap
- Configure kernel parameters
- Install container runtime
- Install kubeadm and kubelet

## Disable Swap

```bash
sudo swapoff -a
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
```

## Configure Kernel Parameters

Follow the same steps as in [Control Plane Setup](control-plane-setup.md#configure-kernel-parameters).

## Install Container Runtime

Follow the steps in [Container Runtime Setup](container-runtime-setup.md).

## Install Kubernetes Components

Install `kubeadm` and `kubelet`:

```bash
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl

sudo curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update
sudo apt-get install -y kubelet kubeadm
sudo apt-mark hold kubelet kubeadm
```

## Join the Cluster

Use the `kubeadm join` command obtained from the control plane setup to join the worker node to the cluster.

```bash
sudo kubeadm join <control-plane-host>:<control-plane-port> --token <token> \\
    --discovery-token-ca-cert-hash sha256:<hash>
```

Replace `<control-plane-host>`, `<control-plane-port>`, `<token>`, and `<hash>` with the actual values provided by the `kubeadm init` command.
