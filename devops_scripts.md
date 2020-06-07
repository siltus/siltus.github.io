# Kubernetes (k8s)

## Delete all pods in "error" state

```
kubectl get pods | grep Error | awk '{print $1}' | xargs kubectl delete pod
```
