

# 🛠️ Kubernetes Deployment Troubleshooting on Kind (Windows)

This document provides a detailed and professional guide for troubleshooting Kubernetes Deployment issues when working with a local **Kind (Kubernetes in Docker)** cluster on **Windows (PowerShell)**.

---

## 📋 Table of Contents

1. [Prerequisites](#prerequisites)
2. [Creating a Kind Cluster](#creating-a-kind-cluster)
3. [Creating a Namespace](#creating-a-namespace)
4. [Deploying to a Namespace](#deploying-to-a-namespace)
5. [Common Deployment Issues](#common-deployment-issues)
6. [Networking & TLS Errors](#networking--tls-errors)
7. [Pod Not Getting Created](#pod-not-getting-created)
8. [Commands Reference](#commands-reference)

---

## ✅ Prerequisites

Ensure the following are installed and running:

* [Docker Desktop](https://www.docker.com/products/docker-desktop)
* [`kind`](https://kind.sigs.k8s.io/docs/user/quick-start/#installation)
* [`kubectl`](https://kubernetes.io/docs/tasks/tools/)

Install `kind` on Windows (if needed):

```powershell
choco install kind
```

Check if Docker is running:

```powershell
docker ps
```

---

## 🔧 Creating a Kind Cluster

To create a basic cluster:

```powershell
kind create cluster --name kd-cluster
```

Verify cluster is up:

```powershell
kubectl cluster-info --context kind-kd-cluster
kubectl get nodes
```

To delete:

```powershell
kind delete cluster --name kd-cluster
```

---

## 🏷️ Creating a Namespace

To create a namespace:

```powershell
kubectl create namespace kd-deployment
```

Check that it's created:

```powershell
kubectl get namespaces
```

---

## 🚀 Deploying to a Namespace

### Option 1: Specify `namespace` in YAML

```yaml
metadata:
  name: nginx-deployment
  namespace: kd-deployment  # Specify here
```

Apply:

```powershell
kubectl apply -f nginx-deployment.yaml
```

### Option 2: Use `-n` flag in CLI

```powershell
kubectl apply -f nginx-deployment.yaml -n kd-deployment
```

Check resources:

```powershell
kubectl get deployments -n kd-deployment
kubectl get pods -n kd-deployment
```

---

## ❗ Common Deployment Issues

### 1. `0/2` Pods Ready, No Pods Found

Check if pods were even created:

```powershell
kubectl get pods
```

If none found, check deployment description:

```powershell
kubectl describe deployment nginx-deployment
```

#### ✅ Likely Cause: YAML Typo

Example of a wrong spec:

```yaml
sepc:  # ❌ typo
```

Correct form:

```yaml
spec:  # ✅ correct
```

---

## 🔐 Networking & TLS Errors

### Error:

```text
Unable to connect to the server: net/http: TLS handshake timeout
```

### Causes:

* Docker not running
* Kind cluster not started
* Network resource issues

### Fix Steps:

```powershell
docker ps
kind get clusters
docker restart kind-control-plane
kubectl cluster-info
```

---

## 🧪 Pod Not Getting Created (But Deployment Exists)

### Diagnosis Steps:

1. Check pod list:

   ```powershell
   kubectl get pods
   ```

2. Describe the deployment:

   ```powershell
   kubectl describe deployment nginx-deployment
   ```

3. Check for recent events:

   ```powershell
   kubectl get events --sort-by='.metadata.creationTimestamp'
   ```

If no pods are being created, it's usually a malformed `template.spec`.

---

## 🧾 Commands Reference

| Task                | Command                                                         |
| ------------------- | --------------------------------------------------------------- |
| Create Kind Cluster | `kind create cluster --name kd-cluster`                         |
| Delete Kind Cluster | `kind delete cluster --name kd-cluster`                         |
| Create Namespace    | `kubectl create namespace kd-deployment`                        |
| Apply Deployment    | `kubectl apply -f file.yaml -n kd-deployment`                   |
| Get Pods            | `kubectl get pods -n kd-deployment`                             |
| Describe Deployment | `kubectl describe deployment nginx-deployment -n kd-deployment` |
| View Events         | `kubectl get events --sort-by='.metadata.creationTimestamp'`    |

---

## 📌 Final Tips

* Always validate your YAML syntax carefully.
* Use `kubectl describe` and `kubectl get events` as your primary debugging tools.
* Keep your Docker and Kind environment stable — most TLS or connectivity issues stem from local setup.

---

