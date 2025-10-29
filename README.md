# Case Study: Plattner Foundation Email Automation
### Infrastructure Setup & Workflow Implementation

## Overview
This project demonstrates the deployment of a **self-hosted instance of n8n** on **Microsoft Azure**, designed to automate email handling for the Plattner Foundation.  

The main objective was to build a cloud-based automation environment capable of:
- Reading incoming emails  
- Classifying them using a Large Language Model (LLM)  
- Sending automatic customize replies for new applications  
- Logging other inquiries for record keeping  


---

## Azure Infrastructure Setup

| Component | Azure Service | Purpose |
|------------|----------------|----------|
| **Resource Group** | `n8n-demo-rg` | Groups all related Azure resources for this deployment. |
| **Container Registry (ACR)** | `n8nacr25802` | Stores the Docker image of n8n so Azure can securely pull it during deployment. |
| **Container Instance (ACI)** | `n8n-aci` | Runs the n8n container in a managed, serverless environment. |

This is lightweight approch, It’s fully managed, cost-effective, and quick to redeploy.

---

## Deployment Process

1. **Create a Resource Group**
   - A resource group named `n8n-demo-rg` was created to hold all Azure components for this project.

2. **Set Up Azure Container Registry (ACR)**
   - The registry `n8nacr25802` was used to host the official `n8nio/n8n:latest` Docker image.  
   - This allows Azure Container Instances to pull the image securely.

3. **Deploy n8n via Azure Container Instance (ACI)**
   - A container instance `n8n-aci` was created using the stored image.  
   - The container runs n8n on port **5678**, and Azure provides a public endpoint such as:  
     ```
     http://n8n-19021.eastus.azurecontainer.io:5678
     ```
   - Environment variables were configured for authentication:
     - `N8N_BASIC_AUTH_USER`
     - `N8N_BASIC_AUTH_PASSWORD`

---

## Configuration & Setup Instructions

1. **Access Your n8n Instance**
   - Open your browser and navigate to the provided endpoint (e.g., `http://n8n-19021.eastus.azurecontainer.io:5678`).  
   - Log in using your configured credentials.

2. **Import the Workflow**
   - Download the workflow JSON file (`Plattner_Email_Automation_Workflow.json`).  
   - In the n8n dashboard, click **Import Workflow** and upload the **PlattnerCaseStudy.json** file.

3. **Add Required Credentials**
   - Configure the **Gmail Trigger** node using your Gmail API credentials (Google Cloud: create 0Auth client, you need to have Client ID and Client Secret and then have to enable the Gmail API).  
   - Add your **OpenAI API key** in the OpenAI node to enable LLM-based processing.  
   - Adjust email subjects, reply messages, or local log file paths as needed.

4. **Run the Workflow**
   - Activate the Gmail trigger node.  
   - Send a few test emails:
     - “We would like to apply for funding” it should trigger an **automatic reply** as for the case of **New APlication**.  
     - “Can I get an update on our application?”  should save as **log**.  
   - Check your output or local log file to verify proper routing.

---

## Workflow Logic Summary

1. **Gmail Trigger**  
   Captures new incoming emails.  

2. **Extract Fields**  
   Retrieves email subject, body, and sender address.  

3. **LLM Node (Classification as per the given types)**  
   Uses OpenAI to identify:
   - Email category (`New Application`, `Status Update`, `General Question`, or `Irrelevant`)  
   - Organization name, contact person, and project title  

4. **Switch Node**  
   Routes the flow based on the category:
   - **New Application**  LLM generates a reply and then Gmail sends auto-response  
   - **Other categories** Logged locally

5. **Set & File Node (Logging)**  
   Stores non-application emails as structured JSON lines in a local log file.

---


## Summary
This setup delivers a complete solution for the Email Automation, with slight changes you can use it for variety of **Use Cases**:
- **Self-hosted n8n instance on Azure**  
- **Fully functional email automation workflow**  

