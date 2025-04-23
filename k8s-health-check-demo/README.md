# 🩺 Kubernetes Health Check Example (Liveness + Readiness)

This repo demonstrates how to use **liveness** and **readiness probes** in Kubernetes.

---

## 🔧 What It Does

- Starts a pod running Ubuntu
- Writes `/tmp/healthy` file to simulate a healthy state
- Uses `cat /tmp/healthy` for both liveness and readiness checks
- Logs "✅ I am healthy" every 5 seconds

---

## 🚀 How to Use

### 1. Apply the Deployment

```bash
kubectl apply -f probe-deploy.yml
```

### 2. Check Pod Status

```bash
kubectl get pods -w
```

### 3. View Logs

```bash
kubectl logs -f <pod-name>
```

You’ll see:

```
✅ I am healthy
✅ I am healthy
...
```

### 4. Simulate a Health Check Failure

```bash
kubectl exec -it <pod-name> -- rm /tmp/healthy
```

In 10–15s, the **liveness probe will fail**, and Kubernetes will restart the container.

### 5. Check Restart Event

```bash
kubectl get pods
kubectl describe pod <pod-name>
```

Look for events like:

```
Liveness probe failed: cat: /tmp/healthy: No such file or directory
```

### 6. Clean Up

```bash
kubectl delete -f probe-deploy.yml
```

---

## 📚 Resources

- [Kubernetes Probes Docs](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)