# 📦 Kubernetes-Based Microservices Application

This project contains a web application designed with a **microservices architecture** running on **Kubernetes**.  
It is designed for **high availability** and **scalability** on AWS infrastructure.

---

## 📌 Architecture Overview

### 🔸 Components

- **Application Load Balancer (ALB)**  
  Handles external requests and forwards traffic to the **Kubernetes Ingress**.

- **Ingress**  
  Distributes HTTP traffic to relevant services (e.g., **TomcatService**).

- **Tomcat**  
  Hosts the main web application and routes incoming requests to microservices.

- **Microservices**
  - **RMQService**: Forwards traffic to the **RabbitMQ** service.
  - **MCService**: Provides caching via **Memcached**.
  - **DBService**: Connects to the database pod.

- **RabbitMQ and Memcached Pods**
  - Asynchronous messaging (**RabbitMQ**)
  - In-memory caching (**Memcached**)

- **DBPod**
  - Database pod with **persistent storage** using **Amazon EBS** mounted at `/var/lib/mysql`.

- **PersistentVolumeClaim & StorageClass**
  - Defines persistent volume requests and storage configuration with EBS.

- **Secrets**
  - Stores sensitive information such as database username and password.

---

## 🚀 Deployment Steps

1. **Create EKS Cluster**

```bash
eksctl create cluster --name my-cluster --region us-east-1
```
2. Deploy Application Manifests
```bash
kubectl apply -f k8s/
```
3. Deploy Ingress and ALB
```bash
kubectl apply -f ingress.yaml
```
4. Create Kubernetes Secret
```bash
kubectl create secret generic db-secret \
  --from-literal=username=myuser \
  --from-literal=password=mypassword
```
5. Access the Application
```bash
echo "http://$(kubectl get ingress tomcat-ingress -o jsonpath='{.status.loadBalancer.ingress[0].hostname}')"
```
📦 Requirements
- AWS Account

- eksctl

- kubectl

- Docker

- Helm

📬 Contact

For questions or feedback, feel free to contact me via GitHub.
