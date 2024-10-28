# Single Node K3s Installation on Ubuntu 22.04 LTS

This guide provides simple steps to set up a single-node K3s cluster on Ubuntu 22.04 LTS.

## Prerequisites

- Ubuntu 22.04 LTS server
- User with `sudo` privileges
- Internet connectivity

## Installation Steps

### 1. Update the System

Update your system packages to the latest versions.

```bash
sudo apt update && sudo apt upgrade -y
```

### 2. Install K3s

Use the official installation script to install K3s.

```bash
curl -sfL https://get.k3s.io | sh -
```

This will install K3s and start the service automatically.

### 3. Verify the Installation

Check the status of the K3s service.

```bash
sudo systemctl status k3s
```

You should see that the service is active and running.

---

## Testing the Cluster

To ensure that your K3s cluster is up and running, perform the following tests.

### 1. Check Node Status

List all the nodes in the cluster.

```bash
kubectl get nodes
```

You should see output similar to:

```
NAME        STATUS   ROLES                  AGE     VERSION
your-node   Ready    control-plane,master   5m      v1.XX
```

### 2. Deploy a Test Application

Deploy a simple Nginx deployment to test cluster functionality.

```bash
kubectl create deployment nginx --image=nginx
```

### 3. Expose the Deployment

Expose the Nginx deployment via a LoadBalancer service.

```bash
kubectl expose deployment nginx --type=LoadBalancer --port=80 --target-port=80
```

**Note:** K3s includes a built-in service load balancer (`klipper-lb`), which allows the `LoadBalancer` service type to work on a single-node cluster.

### 4. Verify the Service

List the services to get the external IP address.

```bash
kubectl get services
```

You should see output like:

```
NAME         TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)        AGE
kubernetes   ClusterIP      10.43.0.1      <none>         443/TCP        10m
nginx        LoadBalancer   10.43.0.123    <node-ip>      80:31234/TCP   1m
```

**Note:** The `<node-ip>` should be the IP address of your K3s server. If `EXTERNAL-IP` shows `<pending>`, wait a few moments and run the command again.

### 5. Test Access to Nginx

Use `curl` to access the Nginx service using the external IP.

```bash
curl http://<node-ip>:80
```

Replace `<node-ip>` with the external IP address of your node (e.g., `192.168.1.10`).

You should see the default Nginx welcome page HTML.

### 6. Clean Up

Delete the test deployment and service.

```bash
kubectl delete service nginx
kubectl delete deployment nginx
```

## Access K3s from Another Machine (Optional)

If you want to access the cluster from another machine, copy the kubeconfig file.

First, display the kubeconfig file.

```bash
sudo cat /etc/rancher/k3s/k3s.yaml
```

Copy the content and save it to `~/.kube/config` on your local machine.

Replace `127.0.0.1` with the IP address of your K3s server.

## Uninstall K3s (Optional)

If you need to remove K3s, run the uninstall script.

```bash
/usr/local/bin/k3s-uninstall.sh
```

## Conclusion

You have successfully set up and tested a single-node K3s cluster on Ubuntu 22.04 LTS. You can now deploy your Kubernetes workloads on this cluster.
