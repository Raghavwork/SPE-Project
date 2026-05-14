# Medical Chatbot DevOps Project

A comprehensive DevOps implementation for a medical chatbot application featuring automated CI/CD pipeline, containerization, Kubernetes orchestration, and observability stack.

## 🏗️ Architecture Overview

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│     Frontend    │◄──►│     Backend     │◄──►│    MongoDB      │
│   (React/Vue)   │    │   (FastAPI)     │    │   (Local DB)    │ 
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         ▼                       ▼                       ▼
┌────────────────────────────────────────────────────────────────┐
│                    Kubernetes Cluster (Minikube)               │
│  ┌──────────────┐ ┌──────────────┐ ┌──────────────────────────┐│
│  │   Services   │ │ Deployments  │ │     ELK Stack            ││
│  │              │ │              │ │ ┌─────────┬─────────────┐││
│  │ • Frontend   │ │ • Frontend   │ │ │ Elastic │  Logstash   │││
│  │ • Backend    │ │ • Backend    │ │ │ search  │             │││
│  │ • ELK Stack  │ │ • ELK Stack  │ │ │         │  Kibana     │││
│  └──────────────┘ └──────────────┘ │ └─────────┴─────────────┘││
└────────────────────────────────────────────────────────────────┘
```

## 🚀 Features

### **Application Stack**
- **Frontend**: Modern web interface
- **Backend**: FastAPI-based REST API
- **Database**: MongoDB (local deployment)
- **Logging**: Centralized logging with ELK Stack
- **Monitoring**: Application health checks and metrics

### **DevOps Features**
- **Containerization**: Docker multi-stage builds
- **Orchestration**: Kubernetes with Minikube
- **CI/CD**: Jenkins pipeline automation
- **Configuration Management**: Ansible playbooks
- **Auto-scaling**: Horizontal Pod Autoscaler (HPA)
- **Service Discovery**: Kubernetes services
- **Persistent Storage**: PV/PVC for data persistence

## 📁 Project Structure

```
medical-chatbot/
├── backend/                    # Backend application code
│   ├── Dockerfile
│   └── requirements.txt
├── frontend/                   # Frontend application code
│   ├── Dockerfile
│   └── package.json
├── kubernetes/                 # Kubernetes manifests
│   ├── namespace.yaml         # Namespace definition
│   ├── config.yaml           # ConfigMaps and Secrets
│   ├── storage.yaml          # PV/PVC definitions
│   ├── services.yaml         # Service definitions
│   ├── deployments.yaml      # Application deployments
│   └── hpa.yaml             # Horizontal Pod Autoscaler
├── ansible/                   # Ansible automation
│   ├── ansible.cfg           # Ansible configuration
│   ├── inventory/
│   │   └── hosts.yml        # Inventory definition
│   ├── playbooks/
│   │   ├── deploy.yml       # Deployment playbook
│   │   └── rollback.yml     # Rollback playbook
│   └── roles/
│       ├── kubernetes-deploy/
│       └── health-check/
├── Jenkinsfile               # CI/CD pipeline definition
├── docker-compose.yml        # Local development setup
├── filebeat.yml             # Log shipping configuration
├── logstash.conf            # Log processing pipeline
├── port-forward.sh          # Service access script
└── README.md                # This file
```

## 🛠️ Technology Stack

| Component | Technology | Purpose |
|-----------|------------|---------|
| **Container Runtime** | Docker | Application containerization |
| **Orchestration** | Kubernetes/Minikube | Container orchestration |
| **CI/CD** | Jenkins | Automated pipeline |
| **Configuration Management** | Ansible | Infrastructure automation |
| **Frontend** | React/Vue.js | User interface |
| **Backend** | FastAPI/Python | REST API server |
| **Database** | MongoDB | Data persistence |
| **Log Collection** | Filebeat | Log shipping |
| **Log Processing** | Logstash | Log parsing and transformation |
| **Search & Analytics** | Elasticsearch | Log storage and search |
| **Visualization** | Kibana | Log analysis dashboard |
| **Monitoring** | Kubernetes Metrics | Resource monitoring |

## 📋 Prerequisites

### **System Requirements**
- **OS**: Ubuntu 20.04+ or compatible Linux distribution
- **CPU**: Minimum 4 cores (8 cores recommended)
- **RAM**: Minimum 8GB (16GB recommended)
- **Storage**: 60GB+ free disk space

### **Required Software**
```bash
# Docker
sudo apt update && sudo apt install -y docker.io
sudo usermod -aG docker $USER

# kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# Minikube
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

# Jenkins
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update && sudo apt install -y jenkins

# Ansible
sudo apt install -y ansible
ansible-galaxy collection install kubernetes.core

# MongoDB
sudo apt install -y mongodb
```

## 🚀 Quick Start

### **1. Clone Repository**
```bash
git clone https://github.com/Raghavwork/medical-chatbot.git
cd medical-chatbot
```

### **2. Start Infrastructure**
```bash
# Start MongoDB
sudo systemctl start mongod
sudo systemctl enable mongod

# Start Minikube
minikube start --cpus=4 --memory=8000 --disk-size=40g

# Create required directories
minikube ssh -- "sudo mkdir -p /mnt/data/logs /mnt/data/elasticsearch && sudo chmod 777 /mnt/data/logs /mnt/data/elasticsearch"

# Verify setup
minikube status
kubectl cluster-info
```

### **3. Configure Jenkins**
1. **Access Jenkins**: http://localhost:8080
2. **Install required plugins**:
   - Docker Pipeline
   - Git
   - Credentials Binding
3. **Add credentials**:
   - **DockerHub**: `dockerhub-creds` (username/password)
   - **Kubeconfig**: `kubeconfig` (secret file: `~/.kube/config`)

### **4. Deploy Application**

#### **Option A: Jenkins Pipeline (Recommended)**
1. Create new Pipeline job in Jenkins
2. Configure SCM: `https://github.com/Raghavwork/medical-chatbot.git`
3. Run pipeline: **Build Now**

#### **Option B: Manual Deployment**
```bash
# Apply Kubernetes manifests
kubectl apply -f kubernetes/namespace.yaml
kubectl apply -f kubernetes/storage.yaml
kubectl apply -f kubernetes/config.yaml
kubectl apply -f kubernetes/services.yaml
kubectl apply -f kubernetes/deployments.yaml
kubectl apply -f kubernetes/hpa.yaml

# Setup port forwarding
chmod +x port-forward.sh
./port-forward.sh
```

### **5. Access Application**
```bash
# Application endpoints
Frontend:      http://localhost:3000
Backend API:   http://localhost:8000
API Docs:      http://localhost:8000/docs
Elasticsearch: http://localhost:9200
Kibana:        http://localhost:5601
```

## 🔧 Configuration

### **Environment Variables**
| Variable | Description | Default |
|----------|-------------|---------|
| `MONGODB_URL` | MongoDB connection string | `mongodb://host.minikube.internal:27017/medicaldb` |
| `ACCESS_TOKEN_EXPIRE_MINUTES` | JWT token expiry | `60` |
| `SECRET_KEY` | Application secret key | (base64 encoded) |

### **Resource Limits**
```yaml
# Backend resources
requests:
  cpu: 1000m
  memory: 4Gi
limits:
  cpu: 4000m
  memory: 16Gi

# Frontend resources  
requests:
  cpu: 100m
  memory: 128Mi
limits:
  cpu: 200m
  memory: 256Mi
```

### **Auto-scaling Configuration**
```yaml
# HPA Settings
minReplicas: 1
maxReplicas: 5
targetCPUUtilizationPercentage: 70
```

## 🔄 CI/CD Pipeline

### **Pipeline Stages**
1. **Checkout**: Clone repository
2. **Pre-deployment Health Check**: Verify system readiness
3. **Docker Login**: Authenticate with DockerHub
4. **Build & Push Images**: Build and publish Docker images
5. **Prepare Environment**: Setup Minikube directories
6. **Deploy using Ansible**: Execute deployment playbooks
7. **Health Check**: Verify deployment status
8. **Application Tests**: Test application endpoints
9. **Setup Port Forwarding**: Enable service access

### **Pipeline Features**
- ✅ **Parallel builds** for frontend and backend
- ✅ **Health checks** at each stage
- ✅ **Automatic rollback** on failure
- ✅ **Image versioning** with build tags
- ✅ **Zero-downtime deployment**
- ✅ **Comprehensive logging**

## 📊 Monitoring & Observability

### **Application Monitoring**
```bash
# Check pod status
kubectl -n medical-chatbot get pods

# View pod logs
kubectl -n medical-chatbot logs -f deployment/backend
kubectl -n medical-chatbot logs -f deployment/frontend

# Monitor resources
kubectl -n medical-chatbot top pods
kubectl -n medical-chatbot get hpa
```

### **ELK Stack Monitoring**
- **Elasticsearch**: Index management and cluster health
- **Logstash**: Log processing pipeline monitoring  
- **Kibana**: Log visualization and dashboard creation
- **Filebeat**: Log collection from application containers

### **Custom Health Checks**
```bash
# Backend health
curl http://localhost:8000/health

# Frontend accessibility
curl http://localhost:3000

# Database connectivity
curl http://localhost:8000/db-status
```

## 🛡️ Security Features

### **Container Security**
- Non-root user execution
- Multi-stage Docker builds
- Minimal base images
- Security context definitions

### **Kubernetes Security**
- Namespace isolation
- Resource quotas and limits
- Secret management for sensitive data
- Service account configurations

### **Network Security**
- ClusterIP services (internal communication)
- Port forwarding for external access
- Network policies (can be extended)

## 🚨 Troubleshooting

### **Common Issues**

#### **Minikube Issues**
```bash
# Minikube not starting
minikube delete && minikube start --cpus=4 --memory=8000

# Permission denied errors
sudo chown -R $USER:$USER ~/.minikube
sudo chmod -R 755 ~/.minikube
```

#### **Pod Issues**
```bash
# Pod stuck in Pending
kubectl -n medical-chatbot describe pod <pod-name>
kubectl -n medical-chatbot get events

# Image pull errors
minikube image load <image-name>
```

#### **Jenkins Issues**
```bash
# Restart Jenkins
sudo systemctl restart jenkins

# Check Jenkins logs
sudo journalctl -u jenkins -f
```

### **Debug Commands**
```bash
# Check cluster status
kubectl cluster-info
kubectl get nodes

# Namespace resources
kubectl -n medical-chatbot get all

# Detailed pod information
kubectl -n medical-chatbot describe pods

# Service endpoints
kubectl -n medical-chatbot get endpoints

# Persistent volumes
kubectl -n medical-chatbot get pv,pvc
```

## 📈 Performance Optimization

### **Resource Optimization**
- **CPU requests**: Set based on actual usage patterns
- **Memory limits**: Prevent OOM kills
- **Storage**: Use appropriate storage classes
- **Network**: Optimize service communication

### **Scaling Strategies**
- **HPA**: CPU-based auto-scaling
- **VPA**: Vertical pod auto-scaling (can be added)
- **Cluster scaling**: Node auto-scaling (for cloud deployments)

## 🔮 Future Enhancements

### **Planned Features**
- [ ] **Helm Charts**: Package management
- [ ] **GitOps**: ArgoCD integration
- [ ] **Service Mesh**: Istio implementation
- [ ] **Cloud Migration**: AWS/GCP deployment
- [ ] **Advanced Monitoring**: Prometheus + Grafana
- [ ] **Security Scanning**: Container and code analysis
- [ ] **Backup Strategy**: Automated data backup
- [ ] **Multi-environment**: Dev/Staging/Prod setup

### **Infrastructure Improvements**
- [ ] **Infrastructure as Code**: Terraform modules
- [ ] **Secret Management**: HashiCorp Vault
- [ ] **Certificate Management**: Cert-manager
- [ ] **Load Testing**: Automated performance testing
- [ ] **Disaster Recovery**: Cross-region backup

## 📝 Development Workflow

### **Local Development**
```bash
# Start local development environment
docker-compose up -d

# Run tests
docker-compose exec backend pytest
docker-compose exec frontend npm test

# View logs
docker-compose logs -f
```

### **Contributing Guidelines**
1. Fork the repository
2. Create feature branch: `git checkout -b feature/amazing-feature`
3. Commit changes: `git commit -m 'Add amazing feature'`
4. Push to branch: `git push origin feature/amazing-feature`
5. Open Pull Request

### **Code Standards**
- **Python**: PEP 8 compliance
- **JavaScript**: ESLint configuration
- **Docker**: Multi-stage builds, minimal images
- **Kubernetes**: Resource limits, health checks
- **Ansible**: Idempotent playbooks, proper error handling

## 📞 Support & Contact

### **Documentation**
- **API Documentation**: http://localhost:8000/docs
- **Kubernetes Docs**: https://kubernetes.io/docs/
- **Ansible Docs**: https://docs.ansible.com/

### **Issue Reporting**
- **GitHub Issues**: https://github.com/Raghavwork/medical-chatbot/issues
- **Bug Reports**: Include logs and environment details
- **Feature Requests**: Describe use case and expected behavior

### **Maintainer**
- **Name**: Raghav Goyal
- **GitHub**: [@Raghavwork](https://github.com/Raghavwork)
- **Project**: Medical Chatbot DevOps Implementation

---
## 🙏 Acknowledgments

- **FastAPI** team for excellent framework
- **Kubernetes** community for orchestration platform
- **Elastic** stack for observability solutions
- **Jenkins** community for CI/CD automation
- **Ansible** team for configuration management
- **Docker** for containerization technology

---

> **Note**: This is a comprehensive DevOps project demonstrating modern deployment practices. For production use, additional security hardening, monitoring, and backup strategies should be implemented.
