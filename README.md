# Kubernetes Cluster Demo (Shivansh)

[![License: MIT](https://img.shields.io/github/license/shivansh123sharma/Kubernetes-cluster-demo)](LICENSE)
[![GitHub issues](https://img.shields.io/github/issues/shivansh123sharma/Kubernetes-cluster-demo)](https://github.com/shivansh123sharma/Kubernetes-cluster-demo/issues)

## Table of Contents

- [Introduction](#introduction)  
- [Architecture](#architecture)  
- [Prerequisites](#prerequisites)  
- [Installation & Setup](#installation--setup)  
- [Cluster Creation](#cluster-creation)  
- [Validation & Usage](#validation--usage)  
- [Cleanup](#cleanup)  
- [Contributing](#contributing)  
- [License](#license)  

---

## Introduction

This repository demonstrates how to create a **Kubernetes cluster on AWS** using **KOPS**.  
It’s designed for learners who want to understand:

- How Kubernetes control plane and worker nodes are provisioned  
- How to manage Kubernetes state using S3  
- The infrastructure that KOPS builds under the hood  

---

## Architecture

```mermaid
graph TD
  subgraph AWS
    S3[S3 Bucket<br>(State Store)]
    Kops[Kops CLI<br>on EC2 / Laptop]
    VPC[VPC / Networking]
    IGW[Internet Gateway]
    IGW --> Subnet
    Subnet --> Master[Master<br>Node]
    Subnet --> Worker1[Worker Node 1]
    Subnet --> Worker2[Worker Node 2]
  end

  Kops --> S3
  Kops --> VPC
  VPC --> Master
  VPC --> Worker1
  VPC --> Worker2
