# Student Feedback System - Kubernetes Commands

## 1. Create Kind Cluster

```bash
kind create cluster --config=config.yml
```

## 2. Verify Cluster

```bash
kubectl cluster-info
kubectl get nodes
kubectl get namespaces
```

---

# 3. Create Namespace

```bash
kubectl apply -f namespace.yml
```

Verify:

```bash
kubectl get ns
```

---

# 4. Deploy Application

```bash
kubectl apply -f secret.yml
kubectl apply -f configmap.yml

kubectl apply -f mongo-deployment.yml
kubectl apply -f mongo-service.yml

kubectl apply -f backend-deployment.yml
kubectl apply -f backend-service.yml

kubectl apply -f frontend-deployment.yml
kubectl apply -f frontend-service.yml

kubectl apply -f ingress.yml
```

---

# 5. Verify Resources

```bash
kubectl get all -n student-feedback
```

Check Pods:

```bash
kubectl get pods -n student-feedback
```

Check Deployments:

```bash
kubectl get deployment -n student-feedback
```

Check Services:

```bash
kubectl get svc -n student-feedback
```

Check Ingress:

```bash
kubectl get ingress -n student-feedback
```

---

# 6. Port Forward

## Backend

```bash
kubectl port-forward service/backend-service 5000:5000 -n student-feedback
```

Access:

```
http://localhost:5000
```

---

## Frontend

```bash
kubectl port-forward service/frontend-service 5173:5173 -n student-feedback
```

Access:

```
http://localhost:5173
```

---

# 7. Horizontal Pod Autoscaler (HPA)

Deploy HPA

```bash
kubectl apply -f backend-hpa.yml
kubectl apply -f frontend-hpa.yml
```

Verify

```bash
kubectl get hpa -n student-feedback
```

Describe

```bash
kubectl describe hpa backend-hpa -n student-feedback
kubectl describe hpa frontend-hpa -n student-feedback
```

Top Metrics

```bash
kubectl top nodes
kubectl top pods -n student-feedback
```

---

# 8. Install Helm

```bash
curl -fsSL https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

Verify

```bash
helm version
```

---

# 9. Helm Chart

## Create Helm Chart

```bash
mkdir helm
cd helm

helm create student-feedback
```

## Verify Chart Structure

```bash
tree student-feedback
```

Expected Structure

```text
student-feedback/
├── Chart.yaml
├── values.yaml
├── charts/
└── templates/
```

## Copy Kubernetes Manifests

```bash
cp ../../k8s/*.yml student-feedback/templates/
```

## Validate Helm Chart

```bash
helm lint ./student-feedback
```

## Render Templates

```bash
helm template student-feedback ./student-feedback
```

## Install Helm Chart

```bash
helm install student-feedback ./student-feedback \
-n student-feedback
```

## Verify Release

```bash
helm list -n student-feedback
```

```bash
kubectl get all -n student-feedback
```

## Upgrade Release

```bash
helm upgrade student-feedback ./student-feedback \
-n student-feedback
```

## Uninstall Helm Release

```bash
helm uninstall student-feedback -n student-feedback
```
# 10. Install Prometheus & Grafana

Add Helm Repository

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

Create Namespace

```bash
kubectl create namespace monitoring
```

Install kube-prometheus-stack

```bash
helm install kind-prometheus prometheus-community/kube-prometheus-stack \
--namespace monitoring
```

---

# 11. Verify Monitoring Stack

```bash
kubectl get pods -n monitoring
```

```bash
kubectl get svc -n monitoring
```

---

# 12. Access Prometheus

Check Service Name

```bash
kubectl get svc -n monitoring
```

Port Forward
,,,
kubectl port-forward svc/kind-prometheus-kube-prome-prometheus 9090:9090 -n monitoring

```

Open Browser

```
http://localhost:9090
```

---

# 13. Access Grafana

Check Service Name

```bash
kubectl get svc -n monitoring
```

Port Forward

```bash
kubectl port-forward svc/kind-prometheus-grafana \
3000:80 -n monitoring
```

Open Browser

```
http://localhost:3000
```

Username

```
admin
```

Password

```bash
kubectl get secret -n monitoring kind-prometheus-grafana \
-o jsonpath="{.data.admin-password}" | base64 -d
```

---

# 14. Prometheus Queries

CPU Usage

```promql
sum(rate(container_cpu_usage_seconds_total{namespace="student-feedback"}[5m])) by (pod)
```

Memory Usage

```promql
sum(container_memory_usage_bytes{namespace="student-feedback"}) by (pod)
```

Network Receive

```promql
sum(rate(container_network_receive_bytes_total{namespace="student-feedback"}[5m])) by (pod)
```

Network Transmit

```promql
sum(rate(container_network_transmit_bytes_total{namespace="student-feedback"}[5m])) by (pod)
```

---

# 15. Useful Commands

```bash
kubectl get all -A
```

```bash
kubectl get events -n student-feedback
```

```bash
kubectl logs <pod-name> -n student-feedback
```

```bash
kubectl describe pod <pod-name> -n student-feedback
```

```bash
kubectl delete namespace student-feedback
```

```bash
kind delete cluster
```