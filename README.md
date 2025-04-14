# Kubernetes-Based Microservices Application

Bu proje, Kubernetes üzerinde çalışan mikroservis mimarili bir web uygulamasını içermektedir. Uygulama, AWS ortamında yüksek erişilebilirlik ve ölçeklenebilirlik hedeflenerek tasarlanmıştır.

## Mimaride Yer Alan Bileşenler

### Application Load Balancer (ALB)
- Dış istekleri karşılar.
- Kubernetes Ingress bileşenine yönlendirir.

### Ingress
- HTTP trafiğini uygun servislere (örneğin TomcatService) dağıtır.

### Tomcat
- Ana uygulamanın çalıştığı sunucudur.
- Gelen istekleri ilgili mikroservislere yönlendirir.

### Mikroservisler
- **RMQService**: RabbitMQ servisine trafik yönlendirir.
- **MCService**: Memcache ile önbellekleme hizmeti sunar.
- **DBService**: Veritabanı pod’una servis sağlar.

### RabbitMQ ve Memcache Pod'ları
- Asenkron mesajlaşma (RabbitMQ) ve bellek içi önbellekleme (Memcache) işlemlerini gerçekleştirir.

### DBPod
- Veritabanı pod'udur.
- `/var/lib/mysql` yolu persistent storage ile bağlanır.
- Verileri kaybetmemek için Amazon EBS kullanır.

### PersistentVolumeClaim ve StorageClass
- Kalıcı disk talebini karşılamak için PVC ve SC tanımlıdır.
- EBS ile fiziksel disk sağlanır.

### Secret
- Veritabanı kullanıcı adı ve şifresi gibi gizli bilgiler burada saklanır.

## Uygulamanın Kurulumu

### 1. EKS Kümesini Oluşturun
```bash
eksctl create cluster --name my-cluster --region us-east-1
```
### 2.Uygulama Manifestlerini Uygulayın
```bash
kubectl apply -f k8s/
```
### 3.Ingress ve ALB Kurulumu
```bash
kubectl apply -f ingress.yaml
```
### 4.Secret Oluşturun
```bash
kubectl create secret generic db-secret \
  --from-literal=username=myuser \
  --from-literal=password=mypassword
```
### 5.Uygulamaya Erişim
```bash
echo "http://$(kubectl get ingress tomcat-ingress -o jsonpath='{.status.loadBalancer.ingress[0].hostname}')"
```
### Gereksinimler
- AWS Hesabı
- eksctl
- kubectl 
- Docker
- Helm
