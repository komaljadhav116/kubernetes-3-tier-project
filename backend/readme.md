# Backend Deployment

This directory contains the Kubernetes manifests and Docker configuration for deploying the Spring Boot backend application.

## Prerequisites

* Docker
* Docker Hub account
* Amazon EKS Cluster
* kubectl
* MariaDB database deployed successfully

---

## Step 1: Clone the Repository

```bash
git clone https://github.com/komaljadhav116/CDEC-studentapp.git

cd CDEC-studentapp/backend
```

---

## Step 2: Update Database Configuration

After deploying the MariaDB Service, obtain its ClusterIP.

```bash
kubectl get svc
```

Copy the **ClusterIP** of the MariaDB Service and update the `application.properties` file.

**File:**

```text
backend/src/main/resources/application.properties
```

Update the following properties:

```properties
spring.datasource.url=jdbc:mariadb://<MARIADB_CLUSTER_IP>:3306/<DATABASE_NAME>
spring.datasource.username=<DATABASE_USERNAME>
spring.datasource.password=<DATABASE_PASSWORD>
```

Save the file before building the Docker image.

---

## Step 3: Build the Docker Image

```bash
docker build -t <dockerhub-username>/studentapp-backend:latest .
```

---

## Step 4: Push the Docker Image

Login to Docker Hub.

```bash
docker login -u <username>
```

Push the image.

```bash
docker push <dockerhub-username>/studentapp-backend:latest
```

---

## Step 5: Update the Deployment

Edit `deployment.yaml` and replace the image with your Docker Hub image.

```yaml
image: <dockerhub-username>/studentapp-backend:latest
```

---

## Step 6: Deploy the Backend

```bash
kubectl apply -f service.yaml
kubectl apply -f deployment.yaml
kubectl apply -f hpa.yaml
```

---

## Step 7: Verify the Deployment

```bash
kubectl get pods
kubectl get svc
kubectl get deployment
kubectl get hpa
```

Expected Result:

* Backend pod is **Running**
* Backend Service is created successfully
* Deployment is **Available**
* HPA is created successfully
