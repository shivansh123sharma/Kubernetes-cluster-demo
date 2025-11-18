# 🚀 Kubernetes Cluster Demo using Kops on AWS

This project demonstrates how to create a production-grade Kubernetes cluster on AWS using **Kops**, **S3 remote state**, and **EC2**.  
Perfect for DevOps beginners learning how real companies create Kubernetes clusters.

---

## 📌 Features
- Provision Kubernetes cluster using **Kops**
- Use **S3 Bucket** as remote state storage
- Deploy cluster across AWS availability zones
- Create master & worker nodes
- Learn real-world Kops commands used in production

---

## 🏗️ Architecture Diagram
```
Local Machine (Ubuntu)
        |
        |  AWS CLI + Kops
        |
AWS S3 Bucket  --->  Stores Kops State (kops-shivansh-storage)
        |
AWS EC2 Instances
   ├── Master Node (t2.micro)
   └── Worker Node(s) (t2.micro)
```

---

## 📦 Prerequisites

### ✔️ Install Tools
- AWS CLI  
- kubectl  
- Kops  

### ✔️ IAM Permissions
Your IAM user must have:

- AmazonEC2FullAccess  
- AmazonS3FullAccess  
- IAMFullAccess  
- AmazonVPCFullAccess  

### ✔️ AWS Setup
Create an S3 bucket:
```
kops-shivansh-storage
```

Cluster domain (gossip-based):
```
demok8scluster.k8s.local
```

---

# 🚀 Step-by-Step Kubernetes Cluster Setup

## 1️⃣ Create S3 bucket for Kops state
```bash
aws s3 mb s3://kops-shivansh-storage
```

## 2️⃣ Export environment variable
```bash
export KOPS_STATE_STORE=s3://kops-shivansh-storage
```

## 3️⃣ Create Kubernetes Cluster
```bash
kops create cluster \
  --name=demok8scluster.k8s.local \
  --state=s3://kops-shivansh-storage \
  --zones=us-east-1a \
  --node-count=1 \
  --node-size=t2.micro \
  --master-size=t2.micro \
  --master-volume-size=8 \
  --node-volume-size=8
```

## 4️⃣ Build the cluster (Creates EC2 resources)
```bash
kops update cluster demok8scluster.k8s.local --yes
```

## 5️⃣ Validate Cluster
```bash
kops validate cluster
```

Expected:
```
Your cluster is ready
```

---

# 🧪 Verify Nodes
```bash
kubectl get nodes
```

Expected Output:
```
master   Ready
node     Ready
```

---

# 🛑 Delete Cluster
```bash
kops delete cluster --name demok8scluster.k8s.local --yes
```

---

## 📚 What You Learn
- Full Kubernetes cluster lifecycle with Kops  
- AWS + Kubernetes integration  
- How S3 state store works  
- Real-world DevOps workflow  
- Cluster creation, validation, deletion  

---

Shivansh DevOps Projects – Kubernetes Series
