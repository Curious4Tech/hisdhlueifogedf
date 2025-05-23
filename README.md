## How to Create a Private Endpoint in Azure: A Step-by-Step Guide 

---

**What is a Private Endpoint in Azure?**  
A private endpoint is a network interface that securely connects your Azure virtual network (VNet) to an Azure service (e.g., Azure Storage, SQL Database, or Key Vault) **without exposing it to the public internet**. It uses a private IP address from your VNet, ensuring traffic between your network and the service stays within the Microsoft backbone network.  

**Benefits of Using Private Endpoints:**  
- **Enhanced Security**: Eliminates public exposure of critical resources.  
- **Compliance**: Helps meet data residency and regulatory requirements.  
- **Simplified Networking**: No complex firewall rules or public IPs needed.  
- **Reduced Latency**: Traffic stays within Azure’s private network.  

**Prerequisites**  
1. An active Azure account with Contributor permissions.  
2. A virtual network (VNet) and subnet where the private endpoint will reside.  
3. The target Azure service (e.g., Storage Account, SQL Server) already deployed.  
4. Ensure the service supports private endpoints, check [supported services](https://learn.microsoft.com/en-us/azure/private-link/private-endpoint-overview).  

---

### **Step 1: Log in to the Azure Portal**  
Navigate to the [Azure Portal](https://portal.azure.com/) and sign in with your credentials.  

![image](https://github.com/user-attachments/assets/3f626cd7-0b87-4c34-9a42-bc433234fabe)

---

### **Step 2: Create a Private Endpoint**  
1. In the search bar, type **Private Endpoint** and select **Private Endpoints** under **Services**.

   ![image](https://github.com/user-attachments/assets/120b6cff-a524-4014-b3e0-a8e4304b8a95)

2. Click **+ Create** to start the setup.  

![image](https://github.com/user-attachments/assets/74c02a04-08ba-4016-97f8-43b9957aaf11)


---

### **Step 3: Configure Basics**  
- **Subscription**: Select your subscription.  
- **Resource Group**: Choose an existing group or create a new one.  
- **Name**: Give your private endpoint a unique name (e.g., `pe-blob-dev`).  
- **Region**: Select the region where your VNet resides.  
Click **Next: Resource >**.  

![image](https://github.com/user-attachments/assets/353c3ac0-cce0-4d45-8009-fcbcd2c9e6b7)


---

### **Step 4: Link to the Target Resource**  
- **Subscription** : Select your subscription, same as the previous.
- **Resource Type**: Select the service type (e.g., `Microsoft.Storage/storageAccounts`).
- **Resource**: Choose your existing resource (e.g., your storage account).  
- **Target Sub-resource**: Select the sub-resource type (e.g., `blob` for storage).  
Click **Next: Configuration >**.  

![image](https://github.com/user-attachments/assets/e7c19075-4425-4173-ae36-100c6c8d8b12)


---

### **Step 5: Configure Networking**  
- **Virtual Network**: Select your VNet.  
- **Subnet**: Choose the subnet for the private endpoint (ensure it’s not used by other services like VMs or gateways).  
- **Private IP Configuration**:  
  - **Dynamically allocate IP**: Let Azure assign an IP automatically.  
  - **Custom IP**: Manually assign an unused IP from the subnet (optional).  
Click **Next: DNS >**.

![image](https://github.com/user-attachments/assets/e85de637-1beb-46c8-ab86-27552023870d)


---

### **Step 6: Integrate with Private DNS Zone**  
Azure can automatically create a DNS record to resolve the service’s FQDN to the private IP.  
- **Integrate with private DNS zone**: Check this box.  
- **Subscription/Resource Group**: Match your environment.  
- **Private DNS Zone**: Azure auto-populates this (e.g., `privatelink.blob.core.windows.net`).  
- **Next: Tags >** (optional)

![image](https://github.com/user-attachments/assets/47f43ff3-b841-4496-ac26-600507d3fa0d)

Click  **Next: Review + Create**. Verify all settings, then click **Create**

![image](https://github.com/user-attachments/assets/48bc641f-4834-4b31-8bae-64a297da7c02)


---

### **Step 7: Review and Deploy**  
Deployment typically takes 1-2 minutes.  

![image](https://github.com/user-attachments/assets/a03f0bef-0c1f-4a0d-be2f-83360201c81d)

---

### **Step 8: Verify Connectivity**  
1. Navigate to the private endpoint resource once deployed by clicking on **Go to Resource**

![image](https://github.com/user-attachments/assets/13a4d8f7-a823-471a-8225-34a5f4996b5c)

   
2. Check the **DNS Configuration** tab to confirm the private IP and DNS record.

![image](https://github.com/user-attachments/assets/6627b87b-501b-422b-97fa-d821ae7af312)

3.  Test connectivity from a VM in the same VNet (e.g., `nslookup [service-hostname]` should resolve to the private IP).  

![image](https://github.com/user-attachments/assets/9682183e-22eb-4d56-b63a-42e09d97c0a1)

Same private IP address as before **192.168.10.4**
---

### **Troubleshooting Tips**  
- **Connection issues?** Ensure the VNet/NSG rules allow traffic to the private IP.  
- **DNS not resolving?** Confirm the private DNS zone is linked to the VNet.  
- **Approval required**: For cross-tenant scenarios, check the target resource’s **Private Endpoint Connections** tab.  

---

**Final Thoughts**  
Private endpoints are a game-changer for securing Azure services. By following this guide, you’ve reduced exposure to threats while ensuring seamless, low-latency access. Ready to lock down more resources? Explore [Azure Private Link](https://learn.microsoft.com/en-us/azure/private-link/) for broader use cases.  

Got questions? Drop them in the comments below!  
