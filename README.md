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
