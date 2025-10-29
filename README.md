# Infrastructure Setup — Azure Deployment

## Overview
This project implements a **self-hosted instance of n8n** deployed on **Microsoft Azure**.  
The goal was to self-host, cloud-based automation environment to manage workflows such as email response automation.

---

## Azure Architecture Summary

| Component | Azure Service | Purpose |
|------------|----------------|----------|
| **Resource Group** | `n8n-demo-rg` | Logical container that organizes all related resources for this deployment. |
| **Container Registry (ACR)** | `n8nacr25802` | Stores the n8n Docker image and allows Azure to securely pull it for deployment. |
| **Container Instance (ACI)** | `n8n-aci` | Hosts and runs the n8n container in a serverless, managed environment. |

This setup provides a lightweight and fully managed environment without manual server maintenance.

---

## Deployment Process

1. **Created a Resource Group**
   - A dedicated resource group `n8n-demo-rg` was created to organize all project components.

2. **Configured Azure Container Registry (ACR)**
   - ACR `n8nacr25802` was created to store and manage the Docker image for n8n.  
   - The official n8n image (`n8nio/n8n:latest`) was pulled and pushed to this registry.

3. **Deployed n8n via Azure Container Instances (ACI)**
   - A new ACI container named `n8n-aci` was created.  
   - The container image was pulled directly from the ACR.  
   - The application was exposed on port `5678` with a public endpoint, making the n8n editor accessible.

4. **Access and Security**
   - Accessed via a generated endpoint such as:  
     `http://n8n-19021.eastus.azurecontainer.io:5678`  
   - Basic authentication was configured using environment variables:
     - `N8N_BASIC_AUTH_USER`
     - `N8N_BASIC_AUTH_PASSWORD`
   
---

## ⚙️ Advantages of This Architecture
- **Lightweight and Serverless:** No management required.  
- **Scalable:** Easily redeploy or resize compute capacity on demand.  
- **Secure and Isolated:** All resources contained within a single Azure Resource Group.  
- **Cost-Efficient:** Pay only for the active runtime of the container.  

---

## Summary
This Azure-based infrastructure forms the foundation for the **email automation workflow** system.  
It ensures reliability, maintainability, and security while leveraging Azure-native container management services.
