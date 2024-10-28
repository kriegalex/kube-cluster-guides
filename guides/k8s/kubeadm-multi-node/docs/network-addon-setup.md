# Network Addon Setup

Install a network addon to enable pod-to-pod communication.

In this guide, we use **Flannel**. For compatibility with MetalLB, refer to the [MetalLB Compatibility Matrix](https://metallb.io/installation/network-addons/).

## Install Flannel

### Create Namespace and Set Labels

```bash
kubectl create namespace kube-flannel
kubectl label --overwrite namespace kube-flannel pod-security.kubernetes.io/enforce=privileged
```

### Install Flannel using Helm

Add the Flannel Helm repository:

```bash
helm repo add flannel https://flannel-io.github.io/flannel/
helm repo update
```

Install Flannel with the correct `podCidr`:

```bash
helm install flannel flannel/flannel \\
    --namespace kube-flannel \\
    --set podCidr="10.244.0.0/16"
```

## Verify Installation

Check that the `coredns` pods are running:

```bash
kubectl get pods -n kube-system -o wide
```

You should see `coredns` pods in the `Running` state.
