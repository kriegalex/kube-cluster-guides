# Ingress Controller Setup (NGINX)

Install the NGINX Ingress Controller using Helm.

## Install ingress-nginx

Add the ingress-nginx Helm repository:

```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
```

Install the ingress-nginx controller:

```bash
helm install ingress-nginx ingress-nginx/ingress-nginx \\
    --namespace ingress-nginx --create-namespace \\
    --set controller.service.externalTrafficPolicy=Local
```

---

## Install cert-manager (Optional)

Add the cert-manager Helm repository:

```bash
helm repo add jetstack https://charts.jetstack.io
helm repo update
```

Install cert-manager:

```bash
helm install cert-manager jetstack/cert-manager \\
    --namespace cert-manager --create-namespace \\
    --version v1.15.1 \\
    --set installCRDs=true
```

---

## Additional Configuration

Refer to the [Ingress Configuration Guide](https://kubernetes.github.io/ingress-nginx/user-guide/) for setting up Ingress resources and TLS certificates.
