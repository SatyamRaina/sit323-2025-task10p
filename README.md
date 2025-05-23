# SIT323/737-2024-T1-Prac10.1P: GKE Deployment & Monitoring of Node.js App

This project demonstrates the deployment of a simple Node.js application to Google Kubernetes Engine (GKE) using Docker and Kubernetes. It includes steps for containerization, deployment, external access configuration, and monitoring. The task aligns with SIT323 Task 10.1P requirements.

---

## 📁 Project Structure
```
├── Dockerfile # Defines container image for Node.js app
├── k8s-deployment.yml # Kubernetes deployment configuration
├── package.json # Node.js dependencies and scripts
├── package-lock.json # Dependency lock file
├── test.js # Calculator microservice (Express + MongoDB)
└── README.md # Project report and deployment instructions
```


---

## ✅ Steps Followed for Deployment

1. **Docker Image Build and Push**
   ```bash
   docker build -t simple-node-app .
   docker tag simple-node-app satyamraina/simple-node-app
   docker push satyamraina/simple-node-app
2. **Authenticate with GCP**
   ```bash
    gcloud auth login
    gcloud config set project sit323-25t1-raina-fbe502e
    gcloud container clusters get-credentials simple-node-cluster --region australia-southeast2
3. Deploy to Kubernetes
   ```bash
    kubectl apply -f k8s-deployment.yaml
    kubectl expose deployment simple-node-app --type=LoadBalancer --port=80 --target-port=3000
4. Check Pod and Service
   ```bash
    kubectl get pods
    kubectl get svc
5. Test API in Postman
  Use the following format:
```bash
http://<external-ip>/add?n1=44&n2=5
```
📊 Monitoring Approach

1. Primary Monitoring (Terminal-Based)
Due to limited IAM permissions in the provided GCP student account, full Metrics Explorer functionality was not accessible. Instead, monitoring was achieved via:
```
kubectl top pods
kubectl top nodes
```
This allowed tracking of CPU and memory utilization in real time.

2. Attempted GCP Monitoring
Metrics Explorer was accessed via the GCP Console, but selecting "Kubernetes Container" metrics failed due to lack of permission:

3. PermissionDenied: permission "monitoring.timeSeries.create" denied
   
⚙️ Tools & Configurations

1. Node.js + Express – Simple arithmetic service.
2. Docker – Containerized the application.
3. Docker Hub – Hosted the container image.
4. GKE (Google Kubernetes Engine) – Deployment platform.
5. kubectl – Kubernetes CLI tool.
6. Postman – Used for endpoint testing.
7. Cloud Run – Alternative deployment method due to LoadBalancer IP issues.
8. Metrics Server – Enabled kubectl top monitoring support.
   
⚠️ Issues & Challenges

1. GCP Monitoring Limitation: Could not access container metrics in GCP due to missing monitoring.metricWriter role. Used kubectl top as a workaround.
2. LoadBalancer EXTERNAL-IP Pending: GKE LoadBalancer IP stayed in pending state.
👉 Used Cloud Run and NodePort as fallback methods.

3. IAM Role Assignment Error: While deploying to Cloud Run, encountered add-iam-policy-binding failure.
👉 Bypassed via --allow-unauthenticated.
