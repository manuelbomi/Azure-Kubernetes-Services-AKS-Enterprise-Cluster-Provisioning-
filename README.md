# Azure Kubernetes Services (AKS) Enterprise Cluster Provisioning

## Introduction  📌 

#### This repository provides a step-by-step guide to provisioning an Azure Kubernetes Service (AKS) cluster using Azure CLI. 

#### The focus is on enterprise-grade applications, where automation, repeatability, and cost-efficiency are critical.

#### While the Azure Portal is often the first entry point for new users, enterprise deployments require a CLI-driven, scriptable, and automated workflows such as shown in figure 1 below, to ensure consistency across environments. 

#### Figure 1: Azure CLI based AKS Cluster Provisioning Workflow

<img width="768" height="155" alt="Image" src="https://github.com/user-attachments/assets/91f2f94c-f867-4f84-a6e4-c44e1a7d1f08" />

---

#### This tutorial documents the full journey of an enterprise grade AKS cluster provisioning: from portal provisioning errors 🠊 to using Azure CLI successfully 🠊 to validating the cluster with kubectl 🠊 and finally connecting the cluster to Lens for observability. 

#### The cluster was designed to provide for multiple pods that can handle an app ( https://app.emmanueloyekanluprojects.com/ ) and its metric monitoring pods ( https://grafana.emmanueloyekanluprojects.com/login   ;   https://prometheus.emmanueloyekanluprojects.com/query  ). 

#### The full step-by-step details regarding how the AKS cluster provisioned with the set of instructions provided here was used to create an enterprise grade app and its monitoring pods are provided at: https://github.com/manuelbomi/End-to-End-LLM-App-Deployment-on-Azure-Kubernetes-Service-AKS-with-Streamlit-Prometheus-Grafana

---

## Why Enterprise Applications Need CLI-Based AKS Provisioning 🏢

* Automation: Enterprise systems require repeatable infrastructure setup. CLI enables scripting and integration with CI/CD pipelines.

* Flexibility: Some VM SKUs are unavailable in certain regions. CLI gives full control to query supported VM sizes dynamically.

* Scalability: Easy to adjust node pools, VM sizes, and regions with CLI scripts.

* Integration: CLI commands can be embedded into enterprise workflows, DevOps pipelines, and Infrastructure-as-Code (IaC).

---

## Azure Portal Limitation  ⚠️ 

#### When creating an AKS cluster in the Azure Portal, you may run into validation errors.

#### Example:

* Attempting to provision a cluster in East US region with Standard_B2ms nodes failed because the VM size was not available in that region.

* This is a common limitation with the portal:

* It doesn’t automatically filter by available SKUs in your subscription + region.

* Enterprise users need a more reliable method: Azure CLI.

#### Example of how this particular issue always occur in practice is shown by the infograph below: 

#### Log onto your Azure portal and select Kubernetes Services
<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/6f83584a-4cf9-4daf-944e-a864237b8269" />

--- 

#### Create Kuberenetes cluster
<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/739277c6-8932-4f87-b72c-119a267bbcc8" />

---

#### Select parameters
<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/7b84aa0c-0a3f-477d-987e-cff59abb83c9" />

---

#### Click on next 
<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/2d3f9012-e92b-40b3-9432-d29e2f696b29" />

---

#### Click next and click add nodepool
<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/f3853d79-f54e-4966-97df-9608829903ba" />

---

#### Select add a virtual machine node pool
<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/d47fbe5d-7da4-4fa2-bbd4-14c5fe392c42" />

---

#### Add node pool 
<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/c263211b-e3b5-4acb-98c0-38fbe5151112" />

---

#### Select node size. Cheapest option B-Series v2 selected here
<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/9ca4625c-2584-40a2-9351-6347f9cd2b44" />

<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/59052c2b-0fc3-4427-9011-201f057f8fba" />

<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/443dbaf6-f96c-40c4-b6e2-c7ba9c2fc723" />

<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/05e3ffb2-47d7-4d8c-94c9-4cc00bc5da31" />

---

#### Click on next until you reach review and create
<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/12dc37cb-95ba-475b-948f-cc1899da7aa3" />

<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/a0e35842-23bb-4de0-85b9-9bbb99f8b6f1" />

<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/3b9219b0-e7bb-458a-99ee-b53439be86bd" />

<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/c7aa7a81-e198-4670-a714-7d8d3c0cce43" />

<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/ebf88c2a-32c2-4cce-b945-862a9a7b405f" />

<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/832d1dd6-2b6a-4361-b273-603e8fdad8cf" />

<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/5690eac2-e205-4991-b35e-e58eb6672ba4" />

<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/13f8c167-692f-45dd-8898-6f50800e3bb3" />

---

#### Error in creating the cluster. 
<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/0e5e0a67-2bca-46be-9cb0-a17d05a171a4" />

---

## Azure CLI Workflow  ✅ 
#### To avoid dashboard issues such as shown above, and to easily provision for enterrpise grade AKS cluster, it is good to use Azure CLI. 

#### Follow the steps below to use Azure CLI to create AKS clusters that can support enterprise grade Kubernetes clusters:

#### 1. List Supported VM Sizes in Your Region

* This command lists all VM SKUs available in your subscription + region, filtering only those with ≥ 2 vCPUs:

---
```ruby
kubectl create namespace ingress-nginx
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install ingress-nginx ingress-nginx/ingress-nginx \
--namespace ingress-nginx \
--set controller.service.type=LoadBalancer

```
---




























