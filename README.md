📌 Project Overview

A full-stack microservices application (Frontend, Backend, MongoDB) deployed on Kubernetes (Kind). Implemented monitoring using Prometheus and Grafana.

🛠 Tech Stack
Kubernetes (Kind / AWS)
Docker
Helm
Prometheus
Grafana
CI/CD (GitHub Actions)
✨ Features
Microservices architecture (Frontend, Backend, MongoDB)
Kubernetes Deployments using YAML files
Service communication using ClusterIP & NodePort
Monitoring using Prometheus + Grafana
Real-time metrics (CPU, Memory, Network usage)
AWS-based deployment (if used)
🚀 How to Run
git clone https://github.com/<your-username>/student-feedback.git
cd student-feedback/k8s
kubectl apply -f .
📊 Monitoring Setup
helm install monitoring prometheus-community/kube-prometheus-stack -n monitoring

📌 Screenshots

Grafana Dashboard
NETWORK

 ![alt text](image.png)

CPU

 ![alt text](image-1.png) 

 Promethus Dashboards

![alt text](image-2.png)

![alt text](image-3.png)


🚀 GitHub Push Commands
git init
git add .
git commit -m "Initial commit - Kubernetes + Monitoring project"
git branch -M main
git remote add origin https://github.com/<username>/student-feedback.git
git push -u origin main
