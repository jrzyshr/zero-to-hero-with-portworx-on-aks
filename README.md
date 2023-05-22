---
marp: true
paginate: true
style: |
    section.lead {
        background-color: #243a5e;
    }
    section.lead h1 {
      color: white;
    }
    section.lead h2 {
      color: white;
    }

---
<!--
_class: lead invert
-->

# Zero to Hero with Portworx on AKS

### Presented by: Tommy Falgout | Cloud Solution Architect

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

![width:800px](https://planomagazine.com/wp-content/uploads/2015/09/SLINGFEST-DFW-TREBUCHET-CATAPULT-PLANO-MAGAZINE-SLIDESHOW.jpg)

---

# Terminology

- K8s - Kubernetes
- AKS - Azure Kubernetes Service
- PX - Portworx
- SP - Service Principal
- MI - Managed Identity

---

# Portworx Permutations

## Kubernetes on Azure

- Self-Managed
- Cluster API / Cluster API for Azure (CAPI / CAPZ)
- Azure Kubernetes Service (AKS)

## Portworx Products

- Portworx Essentials
- Portworx Enterprise
- Portworx Backup
- Portworx Data Services - DBaaS

---

# Azure Kubernetes Service

![width:800px](img/aks.png)

---
# Azure Kubernetes Service

![width:800px](img/aks-2.png)

---

# Portworx Enterprise

- K8s storage platform
- Enterprise grade
- Unlimited volumes
- Free 30 day trial
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

- Kafka
- Elastic
- Cassandra
- TensorFlow
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
kubectl create secret generic -n kube-system px-azure --from-literal=AZURE_CLIENT_ID=$CLIENT_ID

# Install Portworx Operator + spec as per UI Instructions
kubectl apply -f 'https://install.portworx.com/2.13?comp=pxoperator&kbver=1.25.6&ns=portworx'
# Modify 2nd yaml as per Step #9 (remove AZURE_CLIENT_SECRET and AZURE_TENANT_ID)

```

Ref: https://docs.portworx.com/install-portworx/kubernetes/azure/aks/#install-portworx-on-aks-using-the-operator

---

# Scenario: Wordpress on AKS + PX

NOTES:
- Should just need to change SC
- Show screenshots of Azure Portal with added resources

---

# Scenario: ???

---

# Q&A