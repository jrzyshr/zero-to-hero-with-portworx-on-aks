---
theme: microsoft
marp: true
paginate: true
header: ![](img/microsoft-azure-logo-1.png)

---

<!-- 
_class: intro-blue 
_header: ![](img/microsoft-azure-logo-2.png)
-->

# &nbsp;
# Zero to Hero with Portworx on AKS
### Tommy Falgout | Cloud Solution Architect

---

# Agenda

- Intro
- Overview
- Benefits
- How to Deploy
- Scenarios
- Q&A

---
# Me

- Cloud Solution Architect @ Microsoft (ex-Yahoo!, ex-Nortel)
- Builder of trebuchets (IMDB - Trebuchet Expert)
- And amazingly enough, Dad (13 yo daughter)

![bg right:50% w:600](https://planomagazine.com/wp-content/uploads/2015/09/SLINGFEST-DFW-TREBUCHET-CATAPULT-PLANO-MAGAZINE-SLIDESHOW.jpg)

---

# Assumptions and Acronyms

100 level of:
- Azure
- Kubernetes (K8s)
- Portworx (PX)
- Azure Kubernetes Service (AKS)
- Service Principal (SP)
- Managed Identity (MI)

---

![](img/why-k8s.png)

---

![](img/why-aks.png)

---

# Azure Kubernetes Service

![width:800px](img/aks.png)

---
# Azure Kubernetes Service

![width:800px](img/aks-2.png)

---

# Portworx in Azure Permutations

## Kubernetes on Azure

- Self-Managed
- Cluster API / Cluster API for Azure (CAPI / CAPZ)
- Azure Kubernetes Service (AKS)

## Portworx Platform

- Storage Services
- Backup Services
- Data Services - DBaaS

---

# Portworx Enterprise

- K8s storage platform
- Enterprise grade
- Unlimited volumes
- Use Cases
  - DBaaS
  - SaaS with storage
  - Disaster Recovery
  - Cross-Cloud Migration
---

# Benefits of PX on AKS

- Enterprise grade storage platform and managed K8s
- Support Mission critical data support
- Storage pool across clouds
- Storage configuration at pod level to meet SLA
- Backup and migrate across k8s clusters or even clouds!

---

# Data Services

- Microsoft SQL Server (recently announced)
- Kafka
- Elasticsearch
- Cassandra
- MongoDB
- PostgreSQL
- MySQL
- and more

---

# Install steps

```
az group create -n portworx-aks -l southcentralus # 3sec
az aks create -g portworx-aks -n portworx-aks --enable-managed-identity # 3min
az aks get-credentials -g portworx-aks -n portworx-aks # 2sec

# Setup cluster
CLIENT_ID=$(az aks show -g portworx-aks -n portworx-aks --query identityProfile.kubeletidentity.clientId --output tsv)
OBJ_ID=$(az aks show -g portworx-aks -n portworx-aks --query identityProfile.kubeletidentity.objectId --output tsv)
az aks show -g portworx-aks -n portworx-aks --query nodeResourceGroup
MC_RG=$(az aks show -g portworx-aks -n portworx-aks --query nodeResourceGroup --output tsv)
az role assignment create --assignee-object-id $OBJ_ID --role "Contributor" --resource-group $MC_RG # 5 sec
kubectl create secret generic -n portworx px-azure --from-literal=AZURE_CLIENT_ID=$CLIENT_ID

# Install Portworx Operator + spec as per UI Instructions
kubectl apply -f 'https://install.portworx.com/2.13?comp=pxoperator&kbver=1.25.6&ns=portworx'
# Modify 2nd yaml as per Step #9 (remove AZURE_CLIENT_SECRET and AZURE_TENANT_ID)

```

Ref: https://docs.portworx.com/install-portworx/kubernetes/azure/aks/#install-portworx-on-aks-using-the-operator

---

# Scenario: Wordpress on AKS + PX

## 43% of all websites on internet are built on Wordpress

```
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install px-wordpress bitnami/wordpress --set global.storageClass=px-csi-db
```

TODO:
- screenshots/cast + walkthrough

---

# Scenario: Storage Optimization

NOTES:
- Thin provisioning
  - Allocate 5G for Storage
  - Set Autopilot

TODO:
- screenshots/cast + walkthrough

---

# Scenario: BC/DR

NOTES:
- Fail over to separate Region
- Fail over from local env to Cloud

TODO:
- screenshots/cast + walkthrough

---

# Azure Marketplace

<!-- 
Part of K8s Apps in MP.  
Can deploy directly into AKS Cluster
-->

![width:800px](img/portworx-marketplace.png)
![bg right:30% w:300](img/marketplace-qr-code.png)

---
<!-- 
_class: intro-blue centered
_header: ![](img/microsoft-azure-logo-2.png)
-->

# Q & A

---
<!-- 
_class: intro-blue centered
_header: ![](img/microsoft-azure-logo-2.png)
-->

# Thank you