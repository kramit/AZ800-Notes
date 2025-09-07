lab

simple windows admin centre setup

=======

# Module 3 – Windows Server Administration

## Secure Administration in Windows Server

### Least Privilege Administration
- The principle of **least privilege** means giving accounts, services, or processes **only the rights needed** to perform their function — no more.  
- Applied to:  
  - **User accounts** → normal users shouldn’t run as local admins.  
  - **Service accounts** → run with minimal permissions, preferably Managed Service Accounts.  
  - **Processes** → apps/services should not require domain admin to function.  
- Benefits: reduced attack surface, fewer chances of privilege escalation.

---

### Security Principals
- **Users, groups, and computers** are all **security principals** in AD DS.  
- They are identified by **SIDs (Security Identifiers)**.  
- Sensitive administrative groups include **Domain Admins, Enterprise Admins, Schema Admins**.  
- Best practice: minimise memberships in these groups.

---

### Modifying Group Memberships
- Can be controlled with **Group Policy → Restricted Groups**.  
- Ensures sensitive groups don’t accumulate “extra” members over time.  
- Example: A GPO could enforce that only two specific accounts remain members of the “Domain Admins” group.

---

### Determining Currently Assigned Rights
- Rights can be checked in the **Local Security Policy (secpol.msc)** → User Rights Assignment.  
- Important for auditing and confirming least-privilege is enforced.  
- Example: Who has “Log on locally” vs “Log on as a service”?

---

### User Account Control (UAC)
- Adds an extra **elevation prompt** when admin rights are requested.  
- Configurable via Group Policy.  
- Can prompt **admins** differently from **standard users** (e.g., credential prompt vs consent prompt).  
- Helps prevent malware from silently elevating privileges.

---

### Delegated Privileges
- Use the **Delegation of Control Wizard** in ADUC to assign granular permissions.  
- Example: Grant HR staff rights to reset passwords only in the HR OU.  
- Demonstration: delegate “Reset user passwords” to a custom admin group.

---

### Privileged Access Workstations (PAWs)
- **Dedicated, locked-down machines** used only for privileged admin tasks.  
- Must not be used for email, browsing, or general productivity.  
- Two models:  
  - **Dedicated hardware**: One machine for admin, another for day-to-day use.  
  - **Simultaneous use**: Dual-OS setup, one for admin, one for daily tasks.  
- Microsoft recommends **Windows 11 Enterprise** for PAWs.

---

### Jump Servers (Bastion Hosts)
- Hardened servers used as **gateways into secure networks**.  
- Admins connect to the jump server first, then to target systems.  
- In Azure, **Azure Bastion** provides secure RDP/SSH access without exposing VMs to public IPs.  
- Benefits: isolates management plane, reduces exposure.

---

## Windows Server Administration Tools

### Windows Admin Center (WAC)
- Browser-based management tool, acts as a **gateway** using PowerShell remoting/WMI.  
- Benefits:  
  - Easy to deploy, no external dependencies.  
  - Manage servers, clusters, and even Azure resources.  
  - Enhanced security and Azure integration.  
- Demo: Install WAC, add a server, explore Tools pane (certificates, processes, registry).

---

### Server Manager
- Still included in Windows Server.  
- Can manage up to 100 servers remotely.  
- Provides dashboards for:  
  - Adding/removing roles.  
  - Viewing server health.  
  - Configuring local or remote servers.

---

### Remote Server Administration Tools (RSAT)
- RSAT provides admin consoles for AD DS, DNS, DHCP, etc.  
- Allows admins to manage server roles/features from their workstations.

---

### Windows PowerShell
- Core tool for automation and scripting.  
- Cmdlets follow **Verb-Noun** format (e.g., `Get-Service`).  
- Supports remoting:  
  - **One-to-One**: Enter-PSSession.  
  - **One-to-Many**: Invoke-Command.  
  - **SSH remoting**: available cross-platform.  
- Demo: Use `Get-CimInstance` to query a remote server.  
- Important for advanced scenarios like Just Enough Administration (JEA).

---

### PowerShell Remoting – Second Hop Problem

**What is it?**  
- The **second hop problem** occurs when you remote into one server (**Server1**) and then try to connect from there to another resource (**Server2**).  
- By default, Windows does not let your credentials be passed along automatically for that second connection, for security reasons.  
- Example: You connect from Workstation1 → Server1, then try to query a SQL database on Server2. The command fails because your credentials don’t flow through to Server2.

---

#### **Solution 1: CredSSP (Credential Security Support Provider)**
- CredSSP allows your **actual credentials** to be securely delegated from Workstation1 → Server1 → Server2.  
- Makes the second hop possible because Server1 can now use your credentials on your behalf.  
- **Pros**: Simple to configure, quick fix.  
- **Cons**: If Server1 is compromised, your credentials can be stolen. Use only in trusted environments.  

**Configuration Example:**

```powershell
# On Workstation1 (client)
Enable-WSManCredSSP -Role Client -DelegateComputer "Server1.contoso.com" -Force

# On Server1 (server role)
Enable-WSManCredSSP -Role Server -Force

# From Workstation1 → connect to Server1 with CredSSP
Enter-PSSession -ComputerName "Server1.contoso.com" -Authentication CredSSP -Credential (Get-Credential)

# From inside the Server1 session → connect to Server2
Invoke-Command -ComputerName "Server2.contoso.com" -ScriptBlock { hostname }