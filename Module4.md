# Module 4 – Facilitating Hybrid Management

## Remote Management of Windows Server IaaS VMs

### Choosing the Appropriate Remote Administration Tool
When managing Windows Server running in Azure, there are multiple options:
- **Azure Portal** → Basic VM management tasks.  
- **Windows Admin Center (WAC)** → Browser-based management with deep Windows Server integration.  
- **Azure PowerShell** → Scripting and automation for Azure resources.  
- **Azure CLI** → Cross-platform command-line management.  
- **Run Command** → Run scripts directly inside the VM using the VM agent.  
- **Azure Cloud Shell** → Browser-based shell environment with both PowerShell and Bash support.  

*Teaching Tip*: Emphasise that tool choice depends on **scenario** (e.g., WAC for Windows config, CLI for automation, Bastion for secure remote access).

---

### Windows Admin Center (WAC) for IaaS VMs
- WAC provides a **single-pane of glass** to manage both on-premises and cloud servers.  
- To manage an Azure VM with WAC:  
  1. Ensure the VM meets WAC requirements (OS + agent).  
  2. Install WAC inside or connected to the VM.  
  3. Connect via browser to manage roles, certificates, registry, PowerShell, etc.  
- WAC can manage **hybrid workloads** when combined with Azure services (Update Manager, Azure Monitor, Defender for Cloud).

---

### Just-In-Time (JIT) VM Access
- JIT is a **Defender for Cloud feature** that improves security of Azure VMs.  
- Instead of leaving RDP/SSH ports open, JIT:  
  - Blocks all inbound traffic by default.  
  - Opens ports only temporarily when an admin requests access.  
- Admins specify:  
  - Ports allowed.  
  - Allowed source IPs.  
  - Maximum access duration.  
- Benefits: Drastically reduces attack surface (no always-open RDP/SSH).  

**Demo Flow**:  
- Enable JIT on a VM from Defender for Cloud.  
- Request temporary access.  
- Audit activity in Defender for Cloud to see who connected, when, and from where.

---

### Azure Bastion
- **Azure Bastion** provides secure RDP and SSH connectivity to Azure VMs without requiring public IP addresses.  
- Key points:  
  - Deployed into a VNet.  
  - Provides browser-based session directly in the Azure portal.  
  - Bastion host itself has a public IP, but the VMs it protects remain private.  
- Benefits:  
  - Prevents exposing RDP/SSH to the internet.  
  - Supports seamless login using Entra ID or local credentials.  
  - Works with both Windows and Linux VMs.  

**Demo Flow**:  
1. Extend the VM’s virtual network with a `AzureBastionSubnet`.  
2. Deploy Bastion.  
3. From the VM’s “Connect” blade, choose **Bastion**.  
4. Enter credentials and connect securely.  

---

## Managing Hybrid Workloads with Azure Arc

### What is Azure Arc?
- Azure Arc extends **Azure management capabilities** to non-Azure resources (on-premises, other clouds, edge).  
- Supported resource types:  
  - Windows Server  
  - Linux Servers  
  - Kubernetes clusters  
  - SQL Server  

---

### Azure Arc Capabilities
- **Azure Machine Configuration**: Enforce policies on servers.  
- **Log Analytics Integration**: Collect logs/metrics across hybrid estate.  
- **Defender for Cloud**: Extend security controls to non-Azure servers.  
- **Azure Monitor**: Centralised performance and availability monitoring.  
- **Azure Update Manager**: Patch management across servers.  
- **Azure Policy**: Enforce compliance (audit, remediate, deny).

---

### Onboarding Windows Servers to Azure Arc
1. Ensure you have appropriate Entra roles (Connected Machine Onboarding / Connected Resource Admin).  
2. Deploy the **Azure Connected Machine agent** on each server.  
3. Run the installation script from the Azure portal.  
4. Validate the server appears under **Azure Arc > Servers**.  

*Note*: Works for both on-premises and 3rd-party cloud servers (AWS, GCP, etc.).

---

### Managing with Azure Arc
- Once onboarded, you can:  
  - Deploy **VM extensions** (e.g., monitoring, configuration tools).  
  - Apply **Azure Policies** for compliance.  
  - Use **RBAC** to control access.  
- Example: Apply a policy requiring all servers to have Defender AV enabled.  

---

### Role-Based Access Control (RBAC) in Azure Arc
- RBAC provides fine-grained access to hybrid resources managed via Arc.  
- Access control tabs:  
  - **Check access** (see who has permissions).  
  - **Role assignments**.  
  - **Deny assignments**.  
  - **Classic administrators**.  
- Common roles:  
  - Reader  
  - Contributor  
  - Owner  
  - Custom roles (defined via JSON).

---

## Lab 04 – Using Windows Admin Center in Hybrid Scenarios
**Scenario**:  
Contoso wants a **consistent management model** across on-prem and Azure environments.  

**Objectives**:  
- Test connectivity with Azure Network Adapter.  
- Deploy Windows Admin Center gateway in Azure.  
- Verify hybrid functionality of WAC.  
- Connect on-premises and cloud servers to Azure Arc.  
- Apply Azure Policy and monitor compliance.  

---