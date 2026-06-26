# Kubernetes Resources

This document explains the core Kubernetes objects used in a typical 3-tier application (Frontend + Backend + MariaDB) deployed on Kubernetes (EKS).

---

## Deployment

### Purpose
Manages **stateless applications** in Kubernetes.

### Used for
- Frontend (React / Nginx)
- Backend (Java / Node.js microservices)

### Benefits
- Self-healing (automatically recreates failed Pods)
- Rolling updates & rollbacks
- Replica management (ensures desired number of Pods are always running)

---

## StatefulSet

### Purpose
Manages **stateful applications** that require stable identity and persistent storage.

### Used for
- MariaDB database

### Benefits
- Stable Pod names (e.g., mariadb-0, mariadb-1)
- Persistent storage per Pod
- Ordered deployment and scaling
- Ideal for databases and stateful workloads

---

## Service

### Purpose
Provides **stable networking and communication** between Pods.

### Types Used
- ClusterIP (internal communication within cluster)

### Services in Project
- frontend-service
- backend-service
- mariadb-service

### Benefits
- Provides stable DNS name for Pods
- Abstracts Pod IP changes
- Enables reliable service-to-service communication

---

## Ingress

### Purpose
Exposes HTTP/HTTPS applications to external users.

### Benefits
- Single entry point for the cluster
- Path-based routing (e.g., `/api`, `/`)
- Host-based routing (e.g., app.example.com)
- SSL/TLS termination

---

## Secret

### Purpose
Securely stores **sensitive information** in Kubernetes.

### Used for
- Database username
- Database password
- API keys

### Benefits
- Prevents sensitive data exposure in YAML files
- Injected securely into Pods as environment variables or volumes
- Supports RBAC-based access control

---

## PersistentVolumeClaim (PVC)

### Purpose
Requests **persistent storage for MariaDB data** using AWS EBS in EKS.

### Used for
- MariaDB data directory (`/var/lib/mysql`)

### Storage Backend
- AWS EBS (via EBS CSI Driver)
- StorageClass: `ebs-sc`

### Benefits
- Data survives Pod restarts or rescheduling
- Decouples storage from Pod lifecycle
- Ensures database durability and reliability
- Supports dynamic volume provisioning in EKS

### Important Note
PVC works only when:
- AWS EBS CSI driver is installed in the cluster
- StorageClass (`ebs-sc`) is properly configured
- Cluster supports dynamic provisioning

---

## Horizontal Pod Autoscaler (HPA)

### Purpose
Automatically scales Pods based on resource utilization.

### Scaling Metrics
- CPU utilization (primary)
- Memory utilization (optional)

### Benefits
- Handles traffic spikes automatically
- Improves application availability
- Optimizes cost and resource usage

---

## ⭐ Architecture Summary (3-Tier Application)

- **Frontend → Deployment**
- **Backend → Deployment**
- **MariaDB → StatefulSet + PVC**
- **Service → Internal communication**
- **Ingress → External access**
- **HPA → Auto scaling backend**
- **Secret → Secure credential management**