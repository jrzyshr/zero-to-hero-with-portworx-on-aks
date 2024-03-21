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
### Peter Laudati | Cloud Solution Architect
#### Microsoft Corporation

![bg right:25% w:500](img/20240320_164227949_iOS.jpeg)

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

- Cloud Solution Architect @ Microsoft
  - *Building Azure solutions since 2009!*
- Always mixing business with fun =>
- On a challenge to eat a meal in all 564 towns in New Jersey!
  - https://bit.ly/eatjersey

![bg right:45% w:600](img/NJ-StMichel.jpg)

---

# Assumptions and Acronyms

100 level of:
- Azure
- Kubernetes (K8s)
- Portworx (PX)
- Azure Kubernetes Service (AKS)

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


# Portworx Enterprise

![bg right:50% width:600](img/portworx-overview-2.png)

- K8s storage platform
- Enterprise grade
- Use Cases
  - DBaaS
  - SaaS with storage
  - Disaster Recovery
  - Cross-Cloud Migration

---

# Benefits of PX on AKS

![bg right:50% width:600](img/portworx-bcdr.png)

- Enterprise grade storage platform and managed K8s
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

# Install steps (0 to Hero steps)

```
# Create AKS Cluster
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

# AKS + Portworx Architecture

![bg right:65% width:800](img/portworx.png)

---

# Scenario: Wordpress on AKS + PX

### 43% of all websites on internet are built on Wordpress

```
Source: https://www.manaferra.com/wordpress-statistics
```

### Instructions
```
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install px-wordpress bitnami/wordpress \
  --set global.storageClass=px-csi-db
```

![bg right:65% width:800](img/portworx-wordpress.png)

<!--
Example:  Create a company offering WP as a Service.
Want to offer 1TB of storage to each customer
-->
---

# Scenario++: Storage Optimization

![bg right:65% width:800](img/portworx-autopilot.gif)

### Autopilot
- Save on storage costs
- Auto-rescale Portworx storage pools + PVC

<!-- 
Examples:
- Your WP website takes off and people start uploading content, Allocated 1TB, but many customers only using 10GB.
-->

---

# Scenario++: Backup / Disaster Recovery

### Portworx Platform
![bg right:65% width:680](img/portworx-bcdr-2.png)

- Region fail over
- X-cloud or on-prem fail over
- Recovery from snapshop
<!--
Backup is NOT Disaster Recovery
-->
---
# Azure Marketplace

<!-- 
Part of K8s Apps in MP.  
Can deploy directly into AKS Cluster
-->

![width:800px](img/portworx-marketplace.png)
![bg right:30% w:300](img/marketplace-qr-code.png)

---

# Azure Marketplace - K8s Apps

<!-- 
Part of K8s Apps in MP.  
Can deploy directly into AKS Cluster
-->

![bg right:65% width:700](img/k8s-apps-portworx.png)


- New Marketplace Offer
- Deploy into AKS 
- Launch Partner: Portworx
 
---
<!-- 
_class: intro-blue centered
_header: ![](img/microsoft-azure-logo-2.png)
-->

# Q & A

---
<!-- 
_class: centered
_footer: https://lastcoolnameleft.github.io/zero-to-hero-with-portworx-on-aks/
-->

# Thank you

![bg right:30% w:300](img/preso-qr-code.png)

