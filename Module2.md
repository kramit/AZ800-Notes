Lab

TLS 1.2 script

custom domain

idfix

IdFix is a Microsoft tool designed to help organisations prepare their on-premises Active Directory before synchronising it with Microsoft Entra ID (formerly Azure AD). It scans Active Directory objects for issues that would prevent a successful sync and helps remediate them.

quick entra sync

then self service password

then Azure AD Password Protection proxy
 https://learn.microsoft.com/en-us/entra/identity/authentication/concept-password-ban-bad-on-premises


MS Graph module admin


=======


# Active Directory Hybrid Identity – Detailed Notes

## Microsoft Entra ID Overview
Microsoft Entra ID is a **cloud-based identity and access management service**. Unlike traditional AD DS, it is delivered as a **PaaS service** hosted by Microsoft, designed for scalability and high availability. It manages **users, groups, and devices** in the cloud and integrates tightly with Microsoft 365, Azure, and third-party SaaS apps.  
Key differences compared to AD DS:
- **Protocols**: Uses OAuth and OpenID Connect rather than Kerberos or LDAP.  
- **APIs**: Managed via Microsoft Graph API instead of legacy LDAP queries.  
- **Policies**: Relies on Conditional Access rather than Group Policy.  
- **Structure**: No forests or domains; Entra is **multi-tenant** by design. Each organisation has its own tenant, completely isolated from others.  

---

## Integration Models
There are several ways to integrate on-premises AD DS with Microsoft Entra ID:

- **Password Hash Synchronisation (PHS)**  
  Synchronises password hashes from on-premises AD DS to Entra. Users authenticate directly against Entra ID in the cloud. Simple to deploy, but passwords (in hash form) are stored in the cloud.  

- **Pass-Through Authentication (PTA)**  
  Authentication is forwarded to an on-premises AD DS via an authentication agent. Users type credentials into Entra login, but validation happens on-premises. Good balance for organisations not wanting password hashes in the cloud.  

- **Federation (AD FS)**  
  Uses Active Directory Federation Services to issue tokens to Entra. Provides the highest flexibility (custom authentication policies, multi-factor, etc.), but is also the most complex and infrastructure-heavy option.  

Microsoft provides a **decision tree**:  
- Cloud-only? → PHS.  
- Require on-premises validation? → PTA.  
- Complex compliance/customisation? → Federation.  

---

## Directory Synchronisation
**Directory Synchronisation** ensures that objects from on-premises AD DS (users, groups, contacts) are copied to Entra ID. This avoids duplicate account management.  

- **Entra Connect**: Wizard-driven tool that installs on a domain-joined server. Handles synchronisation, optional password writeback, and integration with Exchange hybrid deployments.  
- **Requirements**: Azure Hybrid Identity Administrator in Entra + Enterprise Admin in AD DS.  
- **Health Checks**: Tools like **IdFix** are used to resolve attribute errors (e.g., invalid UPNs).  

---

## Entra Connect vs Cloud Sync
- **Entra Connect**: Full-featured server application, handles large/complex topologies (multiple forests, custom filtering).  
- **Entra Connect Cloud Sync**: Lightweight agent, configuration done in the cloud. Simpler to deploy and maintain but fewer features. Useful for smaller or more agile organisations.  

---

## Seamless Single Sign-On (SSO)
Seamless SSO allows users logged onto a domain-joined device to access Entra-integrated services without typing credentials again.  

- Works with both **PHS** and **PTA**.  
- Uses a feature called **Kerberos decryption keys** stored in Entra.  
- Transparent for the user; no pop-up login prompts when accessing Microsoft 365 apps.  
- *Example*: A user opens Outlook or Teams and is automatically signed in with their AD DS credentials.  

---

## Microsoft Entra Domain Services (Entra DS)
**Entra DS** provides **domain services in the cloud** without deploying domain controllers. It offers features such as **Kerberos authentication, NTLM, Group Policy, and domain joining** for VMs in Azure.  

- Fully managed by Microsoft – you don’t patch or maintain DCs.  
- Great for organisations moving to the cloud but still running apps that need LDAP or legacy authentication.  
- Limitations: flat OU structure, no schema extensions, restricted admin rights (no Enterprise Admin or Schema Admin).  
- Use case: Join Azure VMs to a managed domain and apply GPOs without running your own DCs.  

---

## Deploying AD DS on Azure IaaS VMs
Organisations may choose to run traditional AD DS domain controllers inside Azure VMs.  
Key considerations:
- **Networking**: Configure Azure VNet with site topology that mirrors your on-premises setup.  
- **DNS**: Azure DNS cannot handle SRV records needed for AD; must run Windows Server DNS.  
- **IP Addressing**: Assign static IPs to DCs in Azure (not DHCP).  
- **Storage**: Place `NTDS.dit` and SYSVOL on data disks, not the OS disk.  

Two common scenarios:
1. Entirely cloud-hosted AD DS forest.  
2. Hybrid, with DCs both on-premises and in Azure, connected via VPN or ExpressRoute.  

---

## Trust and Integration in Hybrid Identity
Hybrid deployments often involve trusts:  
- **On-prem AD DS → Entra ID** synchronisation for cloud services.  
- **Cross-forest trusts** when multiple domains must integrate before syncing.  
Trusts are critical in determining how authentication flows between environments.  

---

## Lab 02: Hybrid Identity
Hands-on exercise scenario:  
- Prepare Entra ID for integration with AD DS.  
- Run Entra Connect to sync on-prem AD with the cloud.  
- Test synchronisation (user creation on-prem → appears in Entra).  
- Implement password protection and self-service password reset with writeback.  
- Configure Pass-Through Authentication for hybrid sign-ins.  

This lab demonstrates the **end-to-end integration of on-premises AD with Microsoft Entra ID** and gives students practical experience with hybrid identity models.