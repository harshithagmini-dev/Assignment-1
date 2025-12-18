# 🚀 DevOps Intern Assignment-1

✨ This project demonstrates an **end-to-end DevOps deployment pipeline** using a **Flask API**, **Docker**, **Helm**, **Kubernetes (k3s)**, **Traefik Ingress**, **No-IP domain**, and **cert-manager with TLS**.

---

## 🧰 Tools & Technologies Used ⚙️

- 🐍 **Python (Flask)**
- 🐳 **Docker & Docker Hub**
- ☸️ **Kubernetes (k3s)**
- ⎈ **Helm**
- 🌐 **Traefik Ingress Controller**
- 🔐 **cert-manager**
- 🌍 **No-IP (Dynamic DNS)**
- 🔒 **Let’s Encrypt TLS**

---

## 📌 Task 1: Create a Simple Flask API 🐍

### ✅ Steps
1. Created a Flask application with a single HTTP endpoint.
2. The API returns a JSON message when accessed.

### 📁 File
api/app.py


### ▶️ Test Locally
```bash
python api/app.py
curl http://localhost:8000

📌 Task 2: Containerize and Push to Docker Hub 🐳
✅ Steps

Created a Dockerfile for the Flask application

Built the Docker image locally

Pushed the image to Docker Hub

▶️ Commands
docker build -t harshithagmini8/simple-api:1.0 api/
docker push harshithagmini8/simple-api:1.0

📦 Docker Hub Image
harshithagmini8/simple-api:1.0

📌 Task 3: Create Helm Chart ⎈
✅ Steps

Initialized Helm chart structure

Created Kubernetes Deployment and Service templates

Configured image, replicas, and ports using values.yaml

📁 Files
helm/simple-api/
├── Chart.yaml
├── values.yaml
└── templates/
    ├── deployment.yaml
    ├── service.yaml
    └── ingress.yaml

🔍 Validate Helm Chart
helm lint helm/simple-api

📌 Task 4: Deploy Application to k3s Cluster ☸️
✅ Steps

Verified k3s cluster access

Deployed the application using Helm

Verified pods and services status

▶️ Commands
kubectl get nodes
helm install simple-api helm/simple-api
kubectl get pods
kubectl get svc

📌 Task 5: Configure Traefik Ingress with Domain & TLS 🌐🔐
🛠 Setup Used

k3s default Traefik Ingress Controller

No-IP Dynamic DNS

cert-manager with Let’s Encrypt

🔹 Step 5.1: Configure No-IP Domain 🌍

Created a hostname using No-IP

Pointed the domain to the public IP of the k3s server

📌 Example Domain:

newdomain.ddns.net

🔹 Step 5.2: Install cert-manager 🔐
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/latest/download/cert-manager.yaml


✅ Verify:

kubectl get pods -n cert-manager

🔹 Step 5.3: Create ClusterIssuer for Let’s Encrypt 📜
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt-prod
spec:
  acme:
    email: harshitha@gmail.com
    server: https://acme-v02.api.letsencrypt.org/directory
    privateKeySecretRef:
      name: letsencrypt-prod
    solvers:
      - http01:
          ingress:
            class: traefik


▶️ Apply:

kubectl apply -f cluster-issuer.yaml

🔹 Step 5.4: Configure Traefik Ingress (Helm) 🚦

Used Traefik ingress annotations

Enabled TLS using cert-manager

Linked domain name via Helm values

✨ Ingress Features:

🌐 Domain-based routing

🔐 Automatic TLS certificate provisioning

🔒 Secure HTTPS access

🔹 Step 5.5: Verify TLS Certificate ✅
kubectl get ingress
kubectl describe ingress simple-api
kubectl get certificate


🎯 Expected Result:

Certificate status: READY = True

TLS secret created automatically

🔹 Step 5.6: Access Application 🌍🚀
https://newdomain.ddns.net


📥 API Response:

{
  "message": "10.23.23 16/12/2025"
}

✅ Final Outcome 🎉

✅ Flask API successfully deployed on Kubernetes

✅ Application exposed using Traefik Ingress

✅ Domain configured using No-IP

✅ TLS enabled using cert-manager & Let’s Encrypt

✅ Secure HTTPS access achieved
