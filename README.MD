# Kubernetes Reference Guide

This repository provides a concise and practical overview of essential Kubernetes concepts, manifests, and CLI commands used for deploying and managing applications inside a Kubernetes cluster.

---

## 📦 Kubernetes Manifests Overview

Kubernetes uses declarative YAML files known as **manifests** to define and manage cluster resources. Below are the most common and important manifest types used in real-world deployments.

### 1. **Namespace**

Used to logically organize and isolate resources within the cluster.

### 2. **ConfigMap**

Stores **non-sensitive** configuration data such as URLs, flags, and service names.

### 3. **Secret**

Stores **sensitive** data such as passwords, API keys, tokens, and TLS certificates.

### 4. **Pod**

Represents a single instance of a container. Not commonly used directly in production because they do not self-heal.

### 5. **Deployment**

Manages Pod lifecycle with capabilities such as rolling updates, rollbacks, replica management, and self-healing.

### 6. **ReplicaSet**

Ensures the desired number of Pods are running. Usually created automatically by a Deployment.

### 7. **Service**

Exposes and load balances traffic between Pods.

* **ClusterIP** – internal communication.
* **NodePort** – exposes a port on each node (not recommended in production).
* **LoadBalancer** – provisions a cloud load balancer.

### 8. **Ingress**

Routes external HTTP/HTTPS traffic into the cluster using an ingress controller (e.g., NGINX, AWS ALB).

### 9. **Horizontal Pod Autoscaler (HPA)**

Automatically scales the number of Pod replicas based on CPU or memory usage.

### 🔧 Other Common Resources

* **ServiceAccount** – provides permissions for Pods to access external services (e.g., AWS IAM via roles).
* **Role, ClusterRole, RoleBinding** – implement RBAC (role-based access control).

---

## 🧰 Essential kubectl Commands

### Apply a manifest

```
kubectl apply -f <filename>
```

### List resources

```
kubectl get pods -n <namespace>
kubectl get svc -n <namespace>
kubectl get deployments -n <namespace>
kubectl get namespaces
kubectl get configmaps -n <namespace>
kubectl get secrets -n <namespace>
kubectl get ingress -n <namespace>
kubectl get hpa -n <namespace>
```

### Logs and terminal access

```
kubectl logs <pod-name> -n <namespace>
kubectl exec -it <pod-name> -- /bin/bash
```

### Describe resources

```
kubectl describe <resource-type> -n <namespace>
```

### Rollout operations

```
kubectl rollout status <deployment-name> -n <namespace>
kubectl rollout history <deployment-name> -n <namespace>
kubectl rollout undo <deployment-name> -n <namespace>
```

### Metrics (requires Metrics Server)

```
kubectl top pods -n <namespace>
```

### Delete resources

```
kubectl delete namespace <namespace>
```

---

## ⚙️ Horizontal Pod Autoscaler (HPA)

The HPA automatically adjusts the number of Pod replicas based on CPU or memory usage.

To use HPA, you must:

1. Create a `hpa.yaml` manifest.
2. Configure the `deployment.yaml` with proper **resource requests and limits**.

---

## ❤️ Liveness & Readiness Probes

For Spring Boot applications, you can expose health check endpoints via Actuator:

* **Liveness Probe**: `/actuator/health/liveness`
  Ensures the container is running properly. If it fails, Kubernetes restarts the container.

* **Readiness Probe**: `/actuator/health/readiness`
  Determines if the Pod is ready to receive traffic.

Both probes must return **HTTP 200 OK**.

---

## 📈 How Auto Scaling Works in Kubernetes

1. **Metrics Server** collects CPU and memory metrics from the nodes and Pods.
2. **HPA** reads aggregated metrics and determines if scaling is necessary.
3. **Controller Manager** updates the Deployment with the new replica count.

---

Feel free to expand or customize this README to better fit your project's structure and requirements.


