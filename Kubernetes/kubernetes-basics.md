# Kubernetes Basics

Introduction to Kubernetes core concepts and architecture.

---

## Key Concepts

| Object | Description |
|---|---|
| **Cluster** | Set of nodes (machines) running Kubernetes |
| **Node** | A physical or virtual machine in the cluster |
| **Pod** | Smallest deployable unit; one or more containers |
| **Deployment** | Manages desired state and rolling updates for Pods |
| **Service** | Stable network endpoint to access a set of Pods |
| **ConfigMap** | Non-sensitive configuration data |
| **Secret** | Sensitive configuration data (base64 encoded) |
| **Namespace** | Virtual cluster to isolate resources |
| **Ingress** | HTTP/HTTPS routing to Services |
| **PersistentVolume** | Storage resource in the cluster |

---

## Architecture Overview

```
┌─────────────────────────────────────────────────────┐
│                   Control Plane                     │
│  ┌──────────────┐  ┌──────────┐  ┌──────────────┐  │
│  │ API Server   │  │ etcd     │  │ Scheduler    │  │
│  └──────────────┘  └──────────┘  └──────────────┘  │
│  ┌──────────────────────────────────────────────┐   │
│  │ Controller Manager                           │   │
│  └──────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────┘
         │                       │
┌────────▼──────┐       ┌────────▼──────┐
│   Worker Node │       │   Worker Node │
│  ┌──────────┐ │       │  ┌──────────┐ │
│  │  kubelet │ │       │  │  kubelet │ │
│  └──────────┘ │       │  └──────────┘ │
│  ┌──────────┐ │       │  ┌──────────┐ │
│  │ kube-    │ │       │  │ kube-    │ │
│  │ proxy    │ │       │  │ proxy    │ │
│  └──────────┘ │       │  └──────────┘ │
│  Pod  Pod Pod │       │  Pod  Pod Pod │
└───────────────┘       └───────────────┘
```

---

## Pod

The smallest unit you deploy. Typically runs one container.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  labels:
    app: web
spec:
  containers:
    - name: web
      image: nginx:1.25
      ports:
        - containerPort: 80
      resources:
        requests:
          cpu: "100m"
          memory: "128Mi"
        limits:
          cpu: "500m"
          memory: "256Mi"
```

---

## Deployment

Manages a ReplicaSet to ensure the desired number of Pods are running.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
        - name: web
          image: nginx:1.25
          ports:
            - containerPort: 80
```

---

## Service

Exposes Pods on a stable network endpoint.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-service
spec:
  selector:
    app: web
  ports:
    - port: 80
      targetPort: 80
  type: ClusterIP      # ClusterIP | NodePort | LoadBalancer
```

---

## ConfigMap & Secret

```yaml
# ConfigMap (non-sensitive)
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_ENV: production
  LOG_LEVEL: info

---
# Secret (sensitive — store values as base64)
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
type: Opaque
data:
  DB_PASSWORD: c3VwZXJzZWNyZXQ=   # echo -n "supersecret" | base64
```

---

## Namespaces

```bash
kubectl get namespaces
kubectl create namespace staging
kubectl config set-context --current --namespace=staging
```

---

## Rolling Updates & Rollbacks

```bash
kubectl set image deployment/web-deployment web=nginx:1.26   # Update image
kubectl rollout status deployment/web-deployment             # Watch rollout
kubectl rollout history deployment/web-deployment            # View history
kubectl rollout undo deployment/web-deployment               # Rollback
kubectl rollout undo deployment/web-deployment --to-revision=2
```

---

## Probes (Health Checks)

```yaml
livenessProbe:
  httpGet:
    path: /health
    port: 8080
  initialDelaySeconds: 15
  periodSeconds: 10

readinessProbe:
  httpGet:
    path: /ready
    port: 8080
  initialDelaySeconds: 5
  periodSeconds: 5
```
