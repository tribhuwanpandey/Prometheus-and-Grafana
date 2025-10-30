#  K8s Kind Voting App

This project demonstrates deploying a **Voting App** on a **Kind-based Kubernetes cluster** hosted on an **AWS EC2 instance**, integrated with **Helm**, **Prometheus**, and **Grafana** for monitoring and observability.

---

##  Project Overview
This app replicates a simple microservice-based voting system where users can vote between two choices (e.g., Cats vs Dogs). It demonstrates:
- Kubernetes setup using **Kind**.
- Application deployment with **kubectl**.
- Monitoring and visualization using **Prometheus** and **Grafana** via **Helm**.

---

# Steps for K8s Kind Voting App

## 1. Creating and Managing Kubernetes Cluster with Kind

```bash
clear
kind create cluster --config=config.yml
kubectl cluster-info --context kind-kind
kubectl get nodes
kind get clusters
```

---

## 2. Installing kubectl

```bash
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin
kubectl version --short --client
```

---

## 3. Managing Docker and Kubernetes Pods

```bash
docker ps
kubectl get pods -A
```

---

## 4. Cloning and Running the Example Voting App

```bash
git clone https://github.com/tribhuwanpandey/Prometheus-and-Grafana.git
cd Prometheus-and-Grafana/voting-app/
kubectl apply -f k8s-specifications/
kubectl get all
```

### Port Forwarding for Services

```bash
kubectl port-forward service/vote 5000:5000 --address=0.0.0.0 &
kubectl port-forward service/result 5001:5001 --address=0.0.0.0 &
```

---

## 5. Managing Files in Example Voting App

```bash
cd ..
cd seed-data/
ls
cat Dockerfile
cat generate-votes.sh
```

---

## 6. Installing HELM

```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

---

## 7. Installing Kube Prometheus Stack

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add stable https://charts.helm.sh/stable
helm repo update
kubectl create namespace monitoring

helm install kind-prometheus prometheus-community/kube-prometheus-stack --namespace monitoring   --set prometheus.service.nodePort=30000   --set prometheus.service.type=NodePort   --set grafana.service.nodePort=31000   --set grafana.service.type=NodePort   --set alertmanager.service.nodePort=32000   --set alertmanager.service.type=NodePort   --set prometheus-node-exporter.service.nodePort=32001   --set prometheus-node-exporter.service.type=NodePort

kubectl get svc -n monitoring
kubectl get namespace
```

### Port Forwarding Prometheus and Grafana

```bash
kubectl port-forward svc/kind-prometheus-kube-prome-prometheus -n monitoring 9090:9090 --address=0.0.0.0 &
kubectl port-forward svc/kind-prometheus-grafana -n monitoring 31000:80 --address=0.0.0.0 &
```

---

## 8. Prometheus Queries

```bash
sum (rate (container_cpu_usage_seconds_total{namespace="default"}[1m])) / sum (machine_cpu_cores) * 100

sum (container_memory_usage_bytes{namespace="default"}) by (pod)

sum(rate(container_network_receive_bytes_total{namespace="default"}[5m])) by (pod)
sum(rate(container_network_transmit_bytes_total{namespace="default"}[5m])) by (pod)
```

---

##  Result & Dashboards

### Voting App Interface
![Voting App](<Screenshot 2025-10-30 185513.png>)

### Prometheus Dashboard
![Prometheus](<Screenshot 2025-10-30 185528-1.png>)

### Voting Result
![Voting Result](<Screenshot 2025-10-30 185809.png>)

### Grafana Login
![Grafana Login](<Screenshot 2025-10-30 191508.png>)

### Grafana Dashboards
![Grafana Dashboard 1](<Screenshot 2025-10-30 194225.png>)
![Grafana Dashboard 2](<Screenshot 2025-10-30 194251.png>)
![Grafana Dashboard 3](<Screenshot 2025-10-30 194314.png>)
![Grafana Dashboard 4](<Screenshot 2025-10-30 194329.png>)
![Grafana Dashboard 5](<Screenshot 2025-10-30 194421.png>)

---

##  Summary

| Component | Purpose |
|------------|----------|
| **Kind** | Lightweight Kubernetes cluster for local and EC2 use |
| **Helm** | Manages Kubernetes deployments and charts |
| **Prometheus** | Collects and monitors cluster metrics |
| **Grafana** | Visualizes metrics in dashboards |
| **Voting App** | Demonstrates microservice deployment |

---



