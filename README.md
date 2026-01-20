# Kubernetes Manifest Files for CRM Microservices

This repository contains the consolidated and most up-to-date Kubernetes manifest files for the CRM microservices platform.

## 📁 Repository Structure

```
k8s/
├── namespace.yaml              # Namespace definition (auchitya-platform)
├── services.yaml              # Core microservices deployments & services
├── kafka.yaml                 # Kafka broker configuration
├── zookeeper.yaml            # Zookeeper configuration
├── ingress.yaml              # Ingress rules for external access
├── monitoring.yaml           # Prometheus configuration
├── monitoring-lb.yaml        # Monitoring load balancer
├── grafana-dashboards.yaml   # Grafana dashboards configuration
├── logging-config.yaml       # Logging configuration
├── fluent-bit.yaml          # Log shipping configuration
├── opensearch.yaml          # Log storage and search
├── services-with-logging.yaml # Services with logging enabled
├── cadvisor.yaml            # Container metrics collection
└── node-exporter.yaml       # Node metrics collection
```

## 🚀 Services Included

### **Core Microservices**
- **Order Service** - Handles order management
- **Payment Service** - Processes payments
- **Stock Service** - Manages inventory
- **Demo UI** - Frontend interface

### **Infrastructure Services**
- **Kafka** - Message broker for event streaming
- **Zookeeper** - Kafka coordination service
- **Kafka UI** - Web interface for Kafka management

### **Observability Stack**
- **Prometheus** - Metrics collection and alerting
- **Grafana** - Metrics visualization and dashboards
- **cAdvisor** - Container resource usage metrics
- **Node Exporter** - Host system metrics
- **Fluent Bit** - Log collection and forwarding
- **OpenSearch** - Log storage and search engine

## 🔧 Deployment

### **Prerequisites**
- Kubernetes cluster (EKS recommended)
- kubectl configured
- NGINX Ingress Controller installed

### **Quick Deploy**
```bash
# Deploy all services
kubectl apply -f k8s/

# Check deployment status
kubectl get pods -n auchitya-platform
kubectl get svc -n auchitya-platform
kubectl get ingress -n auchitya-platform
```

### **Individual Component Deployment**
```bash
# Core infrastructure
kubectl apply -f k8s/namespace.yaml
kubectl apply -f k8s/zookeeper.yaml
kubectl apply -f k8s/kafka.yaml

# Microservices
kubectl apply -f k8s/services.yaml

# Networking
kubectl apply -f k8s/ingress.yaml

# Monitoring (optional)
kubectl apply -f k8s/monitoring.yaml
kubectl apply -f k8s/grafana-dashboards.yaml
```

## 🌐 Access URLs

After deployment, services will be available at:

- **Demo UI**: `http://<LOAD_BALANCER_URL>/`
- **Kafka UI**: `http://<LOAD_BALANCER_URL>/kafka-ui`
- **Grafana**: `http://<LOAD_BALANCER_URL>/grafana`
- **Prometheus**: `http://<LOAD_BALANCER_URL>/prometheus`

### **API Endpoints**
- **Orders API**: `http://<LOAD_BALANCER_URL>/api/orders`
- **Payments API**: `http://<LOAD_BALANCER_URL>/api/payments`
- **Stock API**: `http://<LOAD_BALANCER_URL>/api/stocks`

### **Health Checks**
- **Order Service**: `http://<LOAD_BALANCER_URL>/actuator/order/health`
- **Payment Service**: `http://<LOAD_BALANCER_URL>/actuator/payment/health`
- **Stock Service**: `http://<LOAD_BALANCER_URL>/actuator/stock/health`

## 🏗️ Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Demo UI       │    │  Order Service  │    │ Payment Service │
│   (Frontend)    │    │   (Port 8080)   │    │   (Port 8081)   │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         └───────────────────────┼───────────────────────┘
                                 │
                    ┌─────────────────┐
                    │  Stock Service  │
                    │   (Port 8082)   │
                    └─────────────────┘
                                 │
                    ┌─────────────────┐
                    │      Kafka      │
                    │   (Port 9092)   │
                    └─────────────────┘
                                 │
                    ┌─────────────────┐
                    │   Zookeeper     │
                    │   (Port 2181)   │
                    └─────────────────┘
```

## 📊 Monitoring & Observability

The platform includes comprehensive monitoring:

- **Metrics**: Prometheus + Grafana dashboards
- **Logs**: Fluent Bit → OpenSearch
- **Container Metrics**: cAdvisor
- **Host Metrics**: Node Exporter
- **Message Broker**: Kafka UI for topic monitoring

## 🔄 CI/CD Integration

This repository is designed to work with:
- **ArgoCD** for GitOps deployments
- **Jenkins** for CI/CD pipelines
- **ECR** for container image storage

### **ArgoCD Application Configuration**
```yaml
spec:
  source:
    repoURL: https://github.com/auchitya-cloud/ManifestFilesForCRM
    targetRevision: main
    path: k8s
  destination:
    server: https://kubernetes.default.svc
    namespace: auchitya-platform
```

## 🏷️ Image Tags

Current image versions:
- **demo-ui**: `:working` (fixed nginx configuration)
- **order-service**: `:9`
- **payment-service**: `:9`
- **stock-service**: `:9`

## 🔧 Configuration Notes

### **Namespace**
All services deploy to the `auchitya-platform` namespace.

### **Resource Limits**
Services are configured with appropriate resource requests and limits for cost optimization.

### **Networking**
- Services use ClusterIP for internal communication
- External access via NGINX Ingress Controller
- Load balancers for Grafana and Prometheus

## 🚨 Troubleshooting

### **Common Issues**
1. **Pods in Pending state**: Check if cluster nodes are scaled up
2. **503 Service Unavailable**: Verify service endpoints and namespace configuration
3. **ArgoCD sync issues**: Ensure repository URL and path are correct

### **Useful Commands**
```bash
# Check pod status
kubectl get pods -n auchitya-platform

# View pod logs
kubectl logs -n auchitya-platform <pod-name>

# Describe problematic pods
kubectl describe pod -n auchitya-platform <pod-name>

# Check ingress configuration
kubectl get ingress -n auchitya-platform
```

## 📝 Version History

- **v1.0** - Initial consolidated manifest files
- **v1.1** - Fixed demo-ui nginx configuration for correct namespace
- **v1.2** - Added comprehensive monitoring and logging stack

## 🤝 Contributing

1. Make changes to manifest files
2. Test in development environment
3. Submit pull request with detailed description
4. ArgoCD will automatically sync approved changes

---

**Repository**: https://github.com/auchitya-cloud/ManifestFilesForCRM  
**Team**: Auchitya, Balaji, Tharun  
**Project**: CRM Microservices Platform