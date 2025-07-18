# Monitoring Setup with Helm, Prometheus, and Grafana on K3s

This guide walks through installing Helm, setting up Prometheus and Grafana via the `kube-prometheus-stack` Helm chart, and accessing their dashboards on a K3s cluster.

---

## Prerequisites

- Kubernetes cluster (e.g., [K3s](https://k3s.io))
- `kubectl` configured and working
- Internet access to pull Helm charts

---

## Step-by-Step Instructions

### 1. Install Helm

Download and install Helm v3 using the official installation script:

```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
````

### 2. Add Prometheus Helm Chart Repository

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

### 3. Create Namespace for Monitoring Tools

```bash
kubectl create namespace monitoring
```

### 4. Set the KUBECONFIG for K3s

Ensure `kubectl` is pointed to the correct cluster (K3s path example):

```bash
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
```

### 5. Install kube-prometheus-stack via Helm

This chart installs Prometheus, Grafana, and other monitoring tools.

```bash
helm install prometheus-stack prometheus-community/kube-prometheus-stack --namespace monitoring
```

### 6. Verify Pods

Check that all pods are up and running:

```bash
kubectl get pods -n monitoring
```

---

## Access Grafana Dashboard

### 7. Port-forward Grafana to Localhost

```bash
kubectl port-forward svc/prometheus-stack-grafana -n monitoring 3000:80
```

Visit Grafana at: [http://localhost:3000](http://localhost:3000)

### 8. Get Grafana Admin Password

Retrieve the admin password from the Grafana secret:

```bash
kubectl get secret --namespace monitoring prometheus-stack-grafana \
  -o jsonpath="{.data.admin-password}" | base64 --decode
```

> **Username**: `admin`
> **Password**: *(output from above command)*

---

## Access Prometheus Dashboard

### 9. Port-forward Prometheus to Localhost

```bash
kubectl port-forward svc/prometheus-stack-kube-prometheus-prometheus -n monitoring 9090
```

Visit Prometheus at: [http://localhost:9090](http://localhost:9090)

---
