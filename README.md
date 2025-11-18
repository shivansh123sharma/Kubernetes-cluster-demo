🚀 Kubernetes Zero-to-Hero – KOPS on AWS (Shivansh Edition)

A beginner-friendly, step-by-step Kubernetes project that teaches you how to build your own Kubernetes cluster from scratch using KOPS on AWS EC2.
This project is perfect for hands-on DevOps learning, interview preparation, and real-world cluster deployment experience.

🧩 Table of Contents

Introduction

Project Architecture

Prerequisites

Installation Steps

Create Kubernetes Cluster

Validate the Cluster

Useful Kubernetes Commands

Interview Questions

Cleanup

📘 Introduction

This project explains how to deploy a Kubernetes cluster using:

1 Master Node

2 Worker Nodes

Kops as the orchestration tool

AWS EC2 as compute resources

S3 bucket as the cluster state store

Everything is set up exactly how DevOps engineers deploy clusters in real-world production environments.

🏗️ Project Architecture
                         +------------------------------+
                         |        AWS Route53 (Optional)|
                         +--------------+---------------+
                                        |
                                        v
                         +------------------------------+
                         |   S3 Bucket (State Store)    |
                         | kops-shivansh-storage        |
                         +--------------+---------------+
                                        |
                                        v
          +-------------------------------------------------------+
          |                     KOPS CLI                          |
          |  Running on EC2 instance or your local machine        |
          +----------------------+--------------------------------+
                                |
                                v
          +-------------------------------------------------------+
          |                   AWS Infrastructure                   |
          +----------------------+--------------------------------+
                 |                                   |
                 |                                   |
     +-------------------+                 +-------------------------+
     |   Master Node     |                 |       Worker Nodes      |
     | t2.micro          |                 | 2 × t2.micro instances  |
     | Control Plane     |                 | kubelet + workloads     |
     +-------------------+                 +-------------------------+
                 |                                   |
                 +---------------+  +----------------+
                                 v  v
                           +---------------+
                           |   Kubernetes  |
                           |  (API, ETCD,  |
                           | Scheduler...) |
                           +---------------+

📌 Prerequisites
System Requirements

Ubuntu EC2 or laptop

Python3

AWS CLI

kubectl

kops

AWS IAM Permissions

Your IAM user must have:

AmazonEC2FullAccess

AmazonS3FullAccess

IAMFullAccess

AmazonVPCFullAccess

⚙️ Installation Steps
1️⃣ Update system & install required packages
sudo apt-get update
sudo apt-get install -y ca-certificates curl apt-transport-https

2️⃣ Add Kubernetes apt repo & install kubectl
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update
sudo apt-get install -y kubectl

3️⃣ Install AWS CLI
sudo snap install aws-cli --classic
export PATH="$PATH:/home/ubuntu/.local/bin/"

4️⃣ Install KOPS
curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64

chmod +x kops-linux-amd64
sudo mv kops-linux-amd64 /usr/local/bin/kops

5️⃣ Configure AWS CLI
aws configure

🗄️ Create S3 Bucket (Kops State Store)
aws s3api create-bucket --bucket kops-shivansh-storage --region us-east-1


Enable versioning (required):

aws s3api put-bucket-versioning \
  --bucket kops-shivansh-storage \
  --versioning-configuration Status=Enabled


Export KOPS state store:

export KOPS_STATE_STORE=s3://kops-shivansh-storage

🚀 Create Kubernetes Cluster
1 Master + 2 Worker Nodes
kops create cluster \
  --name=demok8scluster.k8s.local \
  --state=s3://kops-shivansh-storage \
  --zones=us-east-1a \
  --node-count=2 \
  --node-size=t2.micro \
  --control-plane-size=t2.micro \
  --control-plane-volume-size=8 \
  --node-volume-size=8

Build the cluster
kops update cluster --name=demok8scluster.k8s.local --state=s3://kops-shivansh-storage --yes --admin


AWS now creates:

VPC

Subnets

Route tables

Internet gateway

Auto-scaling groups

Master EC2

Worker EC2

🔍 Validate the Cluster
kops validate cluster --state=s3://kops-shivansh-storage
kubectl get nodes
kubectl get pods -A


You should see:

1 Master

2 Worker nodes

All nodes in Ready state

⛳ Useful Kubernetes Commands
kubectl get nodes
kubectl get pods
kubectl get pods -A
kubectl get svc
kubectl describe pod <pod-name>
kubectl apply -f <manifest.yaml>

🎯 Interview Questions
1. What is KOPS?

KOPS is a cluster management tool used to create, update, and delete Kubernetes clusters on AWS.

2. Why does KOPS need an S3 bucket?

To store the cluster’s complete “state” — configs, certificates, manifests, and version history.

3. Difference between kops create cluster and kops update cluster?
Command	Action
kops create cluster	Generates config files in S3
kops update cluster --yes	Actually creates AWS resources
4. How to verify cluster health?
kops validate cluster
kubectl get nodes

5. How do master and worker nodes communicate?

Through AWS VPC private networking using kubelet, API server, and a CNI plugin (Calico).

6. How to delete the cluster?
kops delete cluster --name=demok8scluster.k8s.local --yes

🧹 Cleanup

To avoid AWS charges:

kops delete cluster --name=demok8scluster.k8s.local --state=s3://kops-shivansh-storage --yes
aws s3 rb s3://kops-shivansh-storage --force

⭐ Final Note

This project gives you complete Kubernetes hands-on experience for interviews and real DevOps workflows.
