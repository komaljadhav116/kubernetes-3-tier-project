# Kubernetes Troubleshooting

This document covers common Kubernetes issues and how to debug them in a 3-tier application (Frontend + Backend + MariaDB on EKS).

---

## Pods are Pending

### Possible Causes
- Insufficient CPU
- Insufficient Memory
- Storage unavailable
- Node capacity issue

### Check
```bash
kubectl describe pod <pod-name>
kubectl get nodes
kubectl get pvc
```

---

## Pod CrashLoopBackOff

### Check Logs
```bash
kubectl logs <pod-name>
kubectl describe pod <pod-name>
```

### Common Causes
- Wrong environment variables (DB config, secrets)
- Database connection failure (MariaDB not reachable)
- Application crash or bug
- Invalid image or command

---

## ImagePullBackOff

### Check
```bash
kubectl describe pod <pod-name>
```

### Possible Causes
- Wrong image name
- Wrong image tag
- Private Docker repository without credentials
- Image not pushed to registry

---

## Backend Cannot Connect to Database (MariaDB)

### Verify
```bash
kubectl get svc
kubectl get secrets
kubectl describe secret <secret-name>
```

### Test Connection
```bash
kubectl exec -it <backend-pod> -- sh
```

### Possible Causes
- Wrong service name (mariadb-service)
- Incorrect credentials in Secret
- MariaDB pod not running
- DNS or network issue inside cluster

---

## Ingress Not Working

### Verify
```bash
kubectl get ingress
kubectl describe ingress <ingress-name>
```

### Check
- Ingress controller installed (NGINX/ALB)
- Correct service name and port
- DNS configuration (Route53 if used)
- Correct path or host rules

---

## Horizontal Pod Autoscaler (HPA) Not Scaling

### Verify
```bash
kubectl get hpa
kubectl top pods
```

### Check Metrics Server
```bash
kubectl get pods -n kube-system
```

### Possible Causes
- Metrics server not installed or not running
- CPU requests not defined in Deployment
- No traffic/load on application
- Incorrect HPA configuration

---

## PVC Pending

### Check
```bash
kubectl get pvc
kubectl describe pvc <pvc-name>
kubectl get storageclass
```

### Possible Causes
- No StorageClass defined
- EBS CSI driver not installed (EKS)
- No available PersistentVolume
- Incorrect access mode (ReadWriteOnce mismatch)

---

## Useful Commands

```bash
kubectl get all
kubectl get pods -o wide
kubectl get svc
kubectl get ingress
kubectl get pvc
kubectl get hpa

kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl get events --sort-by=.metadata.creationTimestamp
```