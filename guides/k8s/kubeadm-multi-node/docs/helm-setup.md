# Helm Installation

Helm is a package manager for Kubernetes.

## Download and Install

Download the Helm binary:

```bash
wget https://get.helm.sh/helm-v3.15.0-linux-amd64.tar.gz
```

Extract and install:

```bash
tar -zxvf helm-v3.15.0-linux-amd64.tar.gz
sudo mv linux-amd64/helm /usr/local/bin/helm
```

## Verify Installation

```bash
helm help
```

You should see the Helm help output.
