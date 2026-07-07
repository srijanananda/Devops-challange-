# TASK-5: TechMart Backend Container (Docker + ECR + ECS Fargate)

## 📌 Objective
Containerize a simple TechMart Mobile API webpage using Docker and
nginx, push the image to Amazon ECR, and deploy it as a serverless
container using ECS Fargate.

---

## 🗂️ Project Structure
TASK-5-ECS-Containers/
├── index.html      # TechMart Mobile API webpage
├── Dockerfile        # nginx-based container definition
└── README.md

---

## 🛠️ Tools & Services Used
- **Docker**
- **Amazon ECR** (Elastic Container Registry)
- **Amazon ECS** (Fargate launch type)
- **nginx:alpine** (base image)

---

## 🏗️ Architecture
Local build (Docker) → Amazon ECR (image storage) → ECS Fargate (runtime) → Public IP

---

## 🚀 Steps Performed

### 1. Website
Created `index.html` — "TechMart Mobile API - Serving 24/7 Tech Solutions".

### 2. Dockerfile
```dockerfile
FROM nginx:alpine
RUN rm -rf /usr/share/nginx/html/*
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

### 3. Local Testing
```bash
docker build -t techmart-mobile-api .
docker run -d -p 8080:80 --name techmart-test techmart-mobile-api
```
Verified at `http://localhost:8080` before pushing to AWS.

### 4. Push to ECR
```bash
aws ecr create-repository --repository-name techmart-mobile-api --region ap-south-1

aws ecr get-login-password --region ap-south-1 | \
  docker login --username AWS --password-stdin <account-id>.dkr.ecr.ap-south-1.amazonaws.com

docker tag techmart-mobile-api:latest <account-id>.dkr.ecr.ap-south-1.amazonaws.com/techmart-mobile-api:latest
docker push <account-id>.dkr.ecr.ap-south-1.amazonaws.com/techmart-mobile-api:latest
```

### 5. ECS Setup
- **Cluster:** `techmart-cluster` (Fargate, serverless)
- **Task Definition:** `techmart-mobile-api-task` (0.25 vCPU / 0.5 GB, port 80)
- **Service:** `techmart-mobile-service` (desired count: 1, public IP enabled,
  security group allowing inbound TCP 80)

### 6. Verification
Retrieved the running task's public IP and confirmed the site loads
successfully in browser.

---

## 🌐 Live URL
http://<ECS-TASK-PUBLIC-IP>
*(Note: IP changes if the task restarts — a Load Balancer would be
the production fix, planned as a future task.)*

---

## 🎯 Key SRE Concepts Learned

| Concept | Why it matters |
|---|---|
| Containers vs Lambda | Full runtime control vs managed simplicity — different tradeoffs |
| Docker layer caching | Instruction order in Dockerfile affects build speed |
| ECR token-based auth | Avoids storing long-lived registry credentials |
| Task Definition vs Service | Blueprint vs self-healing running state |
| Fargate | Serverless container hosting — no EC2 patching/capacity planning |
| CPU/memory sizing | Hard limits — under-provisioning crashes tasks, over-provisioning wastes cost |
| Ephemeral task IPs | Motivates need for Load Balancers in production |

---