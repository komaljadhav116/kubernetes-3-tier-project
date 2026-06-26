# Deployment Guide

## Prerequisites

Before deploying the application, ensure the following are installed and configured:

* Amazon EKS Cluster
* kubectl
* AWS CLI
* eksctl
* Helm
* Docker
* Docker Hub account
* NGINX Ingress Controller

---

## Deployment Order

Deploy the application in the following sequence:

1. Install the Amazon EBS CSI Driver
2. Deploy the MariaDB Database
3. Deploy the Spring Boot Backend
4. Deploy the React Frontend
5. Verify the Application

---

## Step 1: Install Amazon EBS CSI Driver

Follow the instructions in:

```text
database/install-ebs-csi-driver.md
```

---

## Step 2: Deploy the MariaDB Database

Follow the instructions in:

```text
database/install-ebs-csi-driver.md
```

---

## Step 3: Deploy the Spring Boot Backend

Follow the instructions in:

```text
backend/readme.md
```

---

## Step 4: Deploy the React Frontend

Follow the instructions in:

```text
frontend/readme.md
```

---

## Step 5: Verify the Deployment

Verify that all Kubernetes resources are running successfully.

```bash
kubectl get pods

kubectl get svc

kubectl get ingress

kubectl get pvc

kubectl get statefulset

kubectl get deployment

kubectl get hpa
```

---

## Expected Result

* Amazon EBS CSI Driver is running.
* MariaDB pod is in the **Running** state.
* PersistentVolumeClaim status is **Bound**.
* Spring Boot backend pods are running.
* React frontend pods are running.
* Horizontal Pod Autoscaler (HPA) is created.
* NGINX Ingress is configured successfully.
* The application is accessible through the configured Route 53 domain.
