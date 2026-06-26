# Install Amazon EBS CSI Driver

## Overview

This project uses **Amazon EBS** for persistent storage. Before deploying the MariaDB database, install the Amazon EBS CSI Driver and then deploy the database resources.

---

## Prerequisites

* Amazon EKS Cluster
* AWS CLI configured
* kubectl
* eksctl
* Helm

Verify your cluster:

```bash
kubectl get nodes
```

---

## Step 1: Associate IAM OIDC Provider

```bash
eksctl utils associate-iam-oidc-provider \
  --region=<your-region> \
  --cluster=<your-cluster-name> \
  --approve
```

---

## Step 2: Create IAM Service Account

```bash
eksctl create iamserviceaccount \
  --name ebs-csi-controller-sa \
  --namespace kube-system \
  --cluster=<your-cluster-name> \
  --region=<your-region> \
  --role-name AmazonEKS_EBS_CSI_DriverRole \
  --attach-policy-arn arn:aws:iam::aws:policy/service-role/AmazonEBSCSIDriverPolicy \
  --approve \
  --override-existing-serviceaccounts
```

---

## Step 3: Add the Helm Repository

```bash
helm repo add aws-ebs-csi-driver https://kubernetes-sigs.github.io/aws-ebs-csi-driver

helm repo update
```

---

## Step 4: Install the Amazon EBS CSI Driver

```bash
helm upgrade --install aws-ebs-csi-driver \
aws-ebs-csi-driver/aws-ebs-csi-driver \
--namespace kube-system \
--set controller.serviceAccount.create=false \
--set controller.serviceAccount.name=ebs-csi-controller-sa
```

---

## Step 5: Verify the Installation

```bash
kubectl get pods -n kube-system | grep ebs
```

```bash
kubectl get csidrivers
```

---

## Step 6: Deploy the Database Resources

Apply the database manifests in the following order:

```bash
kubectl apply -f secret.yaml
kubectl apply -f sc.yaml
kubectl apply -f pvc.yaml
kubectl apply -f service.yaml
kubectl apply -f statefulset.yaml
```

---

## Step 7: Verify the Deployment

```bash
kubectl get storageclass
kubectl get pvc
kubectl get statefulset
kubectl get pods
kubectl get svc
```

Expected Result:

* EBS CSI Driver pods are **Running**
* StorageClass is created successfully
* PVC status is **Bound**
* MariaDB StatefulSet is **Ready**
* MariaDB pod is **Running**
