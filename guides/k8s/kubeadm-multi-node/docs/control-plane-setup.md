# Control Plane Setup

This section covers the steps to set up the control plane node.

## Disable Swap

Swap must be disabled for Kubernetes to work properly.

```bash
sudo swapoff -a
```

To make this change permanent, edit `/etc/fstab` and comment out any swap entries:

```bash
sudo nano /etc/fstab
```

Comment out the line containing the swap file or partition.

## Configure Kernel Parameters

Load necessary kernel modules and configure sysctl parameters.

### Load `br_netfilter` Module

```bash
sudo modprobe br_netfilter
```

Ensure the module is loaded on boot:

```bash
echo "br_netfilter" | sudo tee /etc/modules-load.d/k8s.conf
```

### Set Sysctl Parameters

Create `/etc/sysctl.d/k8s.conf` with the following content:

```bash
sudo tee /etc/sysctl.d/k8s.conf <<EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables  = 1
net.ipv4.ip_forward                 = 1
EOF
```

Apply the sysctl settings without reboot:

```bash
sudo sysctl --system
```

### Understanding the Parameters

- **`net.bridge.bridge-nf-call-ip6tables`**: Enables IPv6 bridged traffic to be processed by ip6tables.
- **`net.bridge.bridge-nf-call-iptables`**: Enables IPv4 bridged traffic to be processed by iptables.
- **`net.ipv4.ip_forward`**: Enables IP forwarding between interfaces.

For more information, refer to the [Kernel Parameters Documentation](misc.md#kernel-parameters-explained).

## Install Container Runtime

Install and configure `containerd` as the container runtime.

[See Container Runtime Setup](container-runtime-setup.md)

## Install Kubernetes Components

### Install kubeadm, kubelet, and kubectl

Add the Kubernetes apt repository and install the necessary components.

```bash
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl

sudo curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

### Initialize the Control Plane

Run `kubeadm init` with your desired network CIDR.

```bash
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
```

**Important**: Save the `kubeadm join` command outputted at the end for use when adding worker nodes.

### Configure kubectl Access

Set up the `kubeconfig` file for the `kubectl` command.

```bash
mkdir -p \$HOME/.kube
sudo cp /etc/kubernetes/admin.conf \$HOME/.kube/config
sudo chown \$(id -u):\$(id -g) \$HOME/.kube/config
```

### Allow Scheduling on Control Plane (Optional)

By default, Pods are not scheduled on the control plane node. To allow this (e.g., for single-node clusters):

```bash
kubectl taint nodes --all node-role.kubernetes.io/control-plane-
```

### Install Helm

[See Helm Installation Instructions](helm-setup.md)

### Install Network Addon

Install a network addon to enable pod communication.

[See Network Addon Setup](network-addon-setup.md)
