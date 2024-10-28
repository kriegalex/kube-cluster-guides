# MetalLB Load Balancer Setup

Install MetalLB to provide LoadBalancer services within your cluster.

## Modify kube-proxy ConfigMap

Edit the `kube-proxy` ConfigMap to set `strictARP` to `true`:

```bash
kubectl edit configmap -n kube-system kube-proxy
```

Set the following:

```yaml
apiVersion: kubeproxy.config.k8s.io/v1alpha1
kind: KubeProxyConfiguration
mode: "ipvs"
ipvs:
  strictARP: true
```

---

## Deploy MetalLB

Apply the MetalLB manifests:

```bash
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.14.5/config/manifests/metallb-native.yaml
```

Verify the pods:

```bash
kubectl get pods -n metallb-system
```

---

## Configure IP Address Pool

Create a file named `metallb-config.yaml`:

```yaml
apiVersion: metallb.io/v1beta1
kind: IPAddressPool
metadata:
  name: lan-pool
  namespace: metallb-system
spec:
  addresses:
  - 10.0.1.2-10.0.1.254
---
apiVersion: metallb.io/v1beta1
kind: L2Advertisement
metadata:
  name: lan
  namespace: metallb-system
spec:
  ipAddressPools:
  - lan-pool
```

Apply the configuration:

```bash
kubectl apply -f metallb-config.yaml
```

**Note**: Replace the IP range `10.0.1.2-10.0.1.254` with a range suitable for your network.
