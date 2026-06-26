# Frontend Deployment

This directory contains the Docker configuration and Kubernetes manifests for deploying the React frontend application.

## Prerequisites

* Docker
* Docker Hub account
* Amazon EKS Cluster
* kubectl
* Backend deployed successfully

---

## Step 1: Clone the Repository

```bash
git clone https://github.com/komaljadhav116/CDEC-studentapp.git

cd CDEC-studentapp/frontend
```

---

## Step 2: Update Backend Configuration

After deploying the backend, retrieve the Backend Service details.

```bash
kubectl get svc
```

Retrieve the Worker Node IP.

```bash
kubectl get nodes -o wide
```

Open the `.env` file and update the backend URL.

**File:**

```text
frontend/.env
```

Replace the values with your backend **Node IP** and **NodePort**.

```env
VITE_API_URL=http://<NODE_IP>:<NODE_PORT>/api
```

Example:

```env
VITE_API_URL=http://192.168.1.10:30080/api
```

Save the file before building the Docker image.

---

## Step 3: Build the Docker Image

```bash
docker build -t <dockerhub-username>/studentapp-frontend:latest .
```

---

## Step 4: Push the Docker Image

Login to Docker Hub.

```bash
docker login -u <username>
```

Push the image.

```bash
docker push <dockerhub-username>/studentapp-frontend:latest
```

---

## Step 5: Update the Deployment

Edit `deployment.yaml` and replace the image with your Docker Hub image.

```yaml
image: <dockerhub-username>/studentapp-frontend:latest
```

---

## Step 6: Deploy the Frontend

```bash
kubectl apply -f service.yaml
kubectl apply -f deployment.yaml
kubectl apply -f ingress.yaml
```

---

## Step 7: Verify the Deployment

```bash
kubectl get pods
kubectl get svc
kubectl get ingress
kubectl get deployment
```

Expected Result:

* Frontend pod is **Running**
* Frontend Service is created successfully
* Ingress is created successfully
* Frontend is accessible through the configured Route 53 domain.
