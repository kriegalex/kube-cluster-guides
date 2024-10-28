# Common Commands for Kubernetes Cluster Management

This document provides a quick reference for commonly used Kubernetes (`kubectl`), `k3s`, and storage-related commands. Use this as a guide for managing, troubleshooting, and configuring your Kubernetes clusters efficiently.

---

## Kubernetes (`kubectl`) Commands

### Cluster Information

```
# Check cluster information
kubectl cluster-info

# Get the version of the Kubernetes client and server
kubectl version --short

# View all nodes in the cluster
kubectl get nodes
```

### Namespaces

```
# List all namespaces
kubectl get namespaces

# Switch to a specific namespace
kubectl config set-context --current --namespace=<namespace>

# Create a new namespace
kubectl create namespace <namespace>

# Delete a namespace
kubectl delete namespace <namespace>
```

### Pods and Workloads

```
# List all pods in the current namespace
kubectl get pods

# List all pods in a specific namespace
kubectl get pods -n <namespace>

# View detailed pod information
kubectl describe pod <pod-name>

# Delete a specific pod
kubectl delete pod <pod-name>

# Restart a specific pod (typically by deleting it)
kubectl delete pod <pod-name> --grace-period=0 --force

# List all deployments in the current namespace
kubectl get deployments
```

### Services and Endpoints

```
# List all services in the current namespace
kubectl get services

# Describe a specific service
kubectl describe service <service-name>

# List all endpoints in the current namespace
kubectl get endpoints
```

### Logs and Debugging

```
# View logs for a specific pod
kubectl logs <pod-name>

# View logs for a specific container within a pod
kubectl logs <pod-name> -c <container-name>

# View logs in real-time (follow mode)
kubectl logs -f <pod-name>

# Debug into a pod by starting an interactive shell session
kubectl exec -it <pod-name> -- /bin/sh
```

### ConfigMaps and Secrets

```
# List all ConfigMaps in the current namespace
kubectl get configmaps

# Describe a specific ConfigMap
kubectl describe configmap <configmap-name>

# Create a ConfigMap from a file
kubectl create configmap <configmap-name> --from-file=<file-path>

# List all Secrets in the current namespace
kubectl get secrets

# Create a secret from a literal value
kubectl create secret generic <secret-name> --from-literal=<key>=<value>
```

### Deployment Scaling and Rolling Updates

```
# Scale a deployment
kubectl scale deployment <deployment-name> --replicas=<number>

# Update a deployment image
kubectl set image deployment/<deployment-name> <container-name>=<new-image>

# Roll back to the previous version of a deployment
kubectl rollout undo deployment/<deployment-name>
```

---

## K3s-Specific Commands

### K3s Installation and Status

```
# Install K3s on a master node
curl -sfL https://get.k3s.io | sh -

# Install K3s on a worker node, joining a master node
curl -sfL https://get.k3s.io | K3S_URL=https://<master-ip>:6443 K3S_TOKEN=<node-token> sh -

# Check the K3s service status
sudo systemctl status k3s
```

### K3s Uninstall

```
# Uninstall K3s from a node
sudo /usr/local/bin/k3s-uninstall.sh
```

### K3s CLI Shortcuts

K3s provides a `kubectl` alias; use `k3s kubectl` in place of `kubectl` for cluster commands if `kubectl` isn’t installed.

---

## Storage Commands (Longhorn)

### Longhorn Installation and Management

```
# Install Longhorn via Helm (example command)
helm repo add longhorn https://charts.longhorn.io
helm repo update
helm install longhorn longhorn/longhorn --namespace longhorn-system --create-namespace

# Check the status of Longhorn pods
kubectl -n longhorn-system get pods

# Access the Longhorn UI (port-forwarding example)
kubectl -n longhorn-system port-forward svc/longhorn-frontend 8080:80
```

### Longhorn Volume Management

```
# List all Longhorn volumes
kubectl -n longhorn-system get volumes.longhorn.io

# Create a Longhorn volume (using YAML or Kubernetes spec)

# Attach a Longhorn volume to a specific node
kubectl -n longhorn-system patch volumes.longhorn.io <volume-name> --type merge -p '{"spec":{"nodeID":"<node-id>"}}'

# Detach a Longhorn volume
kubectl -n longhorn-system patch volumes.longhorn.io <volume-name> --type merge -p '{"spec":{"nodeID":""}}'
```

---

## Additional Resources

- **Kubernetes Documentation**: [Kubernetes Documentation](https://kubernetes.io/docs/)
- **K3s Documentation**: [K3s Documentation](https://rancher.com/docs/k3s/latest/en/)
- **Longhorn Documentation**: [Longhorn Documentation](https://longhorn.io/docs/)

---

This guide is designed to cover frequently used commands. For more complex configurations, refer to the official documentation or specific guides in this repository.
