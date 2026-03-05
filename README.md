# 🚀 Azure Kubernetes Service — Containerized REST API

A production-grade containerized Python REST API deployed on Azure Kubernetes Service (AKS) with persistent blob storage, horizontal autoscaling, and Azure Monitor alerting.

---

## 🏗️ Architecture Overview
```
Developer pushes code
        ↓
Docker builds container image
        ↓
Image pushed to Azure Container Registry (ACR)
        ↓
AKS pulls image from ACR
        ↓
Kubernetes deploys it as a Pod
        ↓
Azure Load Balancer exposes public IP
        ↓
Users access the app via browser
        ↓
File uploads persist to Azure Blob Storage
```

---

## 🛠️ Technologies Used

| Technology | Purpose |
|-----------|---------|
| **Python 3.11** | Application runtime |
| **FastAPI** | REST API framework |
| **Uvicorn** | ASGI web server |
| **Docker** | Containerization |
| **Azure Container Registry (ACR)** | Private container image storage |
| **Azure Kubernetes Service (AKS)** | Container orchestration |
| **Azure Blob Storage** | Persistent file storage |
| **Azure Monitor** | Monitoring and alerting |
| **kubectl** | Kubernetes CLI |
| **Azure CLI** | Azure management CLI |

---

## 📁 Project Structure
```
my-azure-app/
├── main.py              # FastAPI application
├── Dockerfile           # Container build instructions
├── requirements.txt     # Python dependencies
├── .dockerignore        # Files excluded from container
├── deployment.yaml      # Kubernetes deployment + service manifest
└── README.md            # This file
```

---

## 🔌 API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/` | GET | Welcome message |
| `/health` | GET | Kubernetes liveness & readiness probe |
| `/info` | GET | Runtime environment info |
| `/items` | GET | List all items |
| `/items/{id}` | GET | Get item by ID |
| `/upload` | POST | Upload file to Azure Blob Storage |
| `/files` | GET | List all files in Blob Storage |


---

## 🚀 Deployment Guide

### Prerequisites
- Azure subscription
- Docker Desktop installed
- Azure CLI installed
- kubectl installed

### Step 1 — Clone the Repository
```bash
git clone https://github.com/YOUR_USERNAME/azure-aks-deployment.git
cd azure-aks-deployment
```

### Step 2 — Build Docker Image
```bash
docker build -t my-azure-app .
```

### Step 3 — Push to Azure Container Registry
```bash
az acr login --name myazureappacr
docker tag my-azure-app myazureappacr.azurecr.io/my-azure-app:v1
docker push myazureappacr.azurecr.io/my-azure-app:v1
```

### Step 4 — Connect to AKS
```bash
az aks get-credentials --resource-group rg-secure-workload-prod --name aks-cluster-my
```

### Step 5 — Create Storage Secret
```bash
kubectl create secret generic storage-secret \
  --from-literal=connection-string="YOUR_STORAGE_CONNECTION_STRING"
```

### Step 6 — Deploy to AKS
```bash
kubectl apply -f deployment.yaml
```

### Step 7 — Verify Deployment
```bash
kubectl get pods
kubectl get services
kubectl get hpa
```

---

## 🔒 Security Configuration

### Kubernetes Secrets
Sensitive credentials stored as Kubernetes secrets — never hardcoded in code or YAML files.

### Private Container Registry
All images stored in private ACR — not publicly accessible.

### Resource Limits
CPU and memory limits defined per pod to prevent resource exhaustion.

### SSL (Planned)
1. Install NGINX Ingress Controller
2. Deploy cert-manager
3. Configure Let's Encrypt
4. Enforce HTTPS via Ingress resource

---

## 📊 Monitoring & Alerting

| Alert | Threshold | Severity |
|-------|-----------|----------|
| High CPU | > 80% | Warning (Sev 2) |
| High Memory | > 80% | Warning (Sev 2) |
| Pod Restarts | > 5 in 15min | Error (Sev 1) |
| Node Not Ready | Any | Critical (Sev 0) |

---

## ⚡ Autoscaling
```
Minimum pods: 1
Maximum pods: 5
Scale up when CPU > 50%
```

---

## 💾 Storage

| Setting | Value |
|---------|-------|
| Storage account | myazureappstorage |
| Container | static-files |
| Redundancy | LRS |
| Access | Private via Kubernetes secret |

---

## 💰 Cost Breakdown

| Resource | Cost |
|----------|------|
| AKS node (D2pds_v5) | ~$65/month |
| ACR Basic | ~$5/month |
| Blob Storage | ~$1/month |
| **Total** | **~$71/month** |

---

## 🔜 Planned Improvements
- [ ] CI/CD pipeline with GitHub Actions
- [ ] SSL certificate with cert-manager + Let's Encrypt
- [ ] Custom domain
- [ ] Terraform for full infrastructure as code
- [ ] Azure Key Vault for secret management

---

## 👤 Author
Built as a Systems & Cloud Engineer portfolio project demonstrating containerization, Kubernetes orchestration, cloud storage, monitoring, and DevOps best practices on Microsoft Azure.
