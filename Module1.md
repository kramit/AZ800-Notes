Lab

Remote install adds on svr1 from adm1

make a user ty

change his password

group policy make a gpo with screen saver settings

group policy show permissions to a GPO to filter

WMI Filter

=============

# Active Directory Core Concepts – Detailed Notes

## Forests, Domains, Trees, and OUs
At the top of the Active Directory hierarchy is the **forest**, which is the ultimate **security and administrative boundary**. A forest contains one or more trees, shares a single schema and global catalog, and defines the trust relationships between domains. Within a forest, a **tree** is a collection of one or more domains that share a contiguous namespace, such as `contoso.com` and its child domain `hr.contoso.com`. A **domain** itself is a logical boundary for managing users, groups, and resources. Domains are often thought of as administrative units with their own policies and replication scope. Inside domains, administrators can create **Organisational Units (OUs)** to structure objects logically. OUs make it possible to delegate administration and apply Group Policies to specific groups of users or computers. For example, an enterprise could have a single forest, several trees for different subsidiaries, multiple domains for regions, and OUs for departments like HR, Sales, or IT.

---

## Global Catalog
The **global catalog (GC)** is a distributed data repository that provides a **partial, read-only copy of every object** in an Active Directory forest. Its primary role is to **speed up searches and authentication** across the forest. For example, when a user in one domain searches for another user in a different domain, the query is answered by the GC without needing to contact every domain controller in the forest. The GC contains commonly searched attributes, such as usernames and group memberships. It is also essential during logon: when a user logs on with a Universal Group membership, the DC consults the GC to confirm access. For resilience, best practice is to ensure that each site has at least one global catalog server available.

---

## NTDS.dit
The **NTDS.dit** file is the **Active Directory database**. Located on each domain controller, it stores all directory information, including user accounts, groups, passwords (in hashed form), and schema definitions. It is a **transactional database**, meaning changes are written first to a transaction log before being committed to the database itself, which helps maintain integrity. By default, NTDS.dit resides in `%systemroot%\NTDS`, but administrators can place it on a different disk for performance or redundancy reasons. Because of its criticality, backup strategies must include the system state, which captures NTDS.dit. Corruption of this file can lead to loss of directory services functionality, making recovery planning vital.

---

## Group Scopes: Local, Domain-Local, Global, Universal
Groups in AD DS are defined not just by type (security or distribution) but also by **scope**, which determines where they can assign permissions.

- **Local groups**: Exist only on individual machines and are rarely used in enterprise AD management.  
- **Domain-Local groups**: Can be granted permissions to resources within their own domain, but their membership can include users and groups from other domains.  
- **Global groups**: Contain users from their own domain only, but can be granted permissions across the entire forest.  
- **Universal groups**: Can contain users and groups from any domain in the forest and can be used to grant permissions anywhere in the forest.  

The classic “AGDLP” (Accounts → Global → Domain-Local → Permissions) model helps administrators manage membership efficiently: users go into Global groups, which are added to Domain-Local groups that are assigned permissions.

---

## FSMO Roles
While AD DS uses a **multi-master replication model** where changes can be made on any domain controller, certain operations must be performed by a **single authoritative controller**. These are the **FSMO (Flexible Single Master Operations) roles**. There are five in total:

- **Schema Master** – controls updates to the schema (forest-wide).  
- **Domain Naming Master** – manages additions or removals of domains in the forest (forest-wide).  
- **RID Master** – allocates blocks of Relative IDs to DCs to ensure unique security identifiers (domain-wide).  
- **PDC Emulator** – provides backward compatibility for NT 4.0 BDCs, processes password changes, and acts as the time source (domain-wide).  
- **Infrastructure Master** – updates cross-domain references (domain-wide).  

Understanding which roles are forest-wide and which are domain-wide is important for troubleshooting, especially during DC migrations or failures.

---

## Administrative Templates (.admx / .adml)
Group Policy relies on **administrative template files** to define configurable settings. These templates come in two parts:

- **.admx files** – language-neutral XML files that define the actual policy settings available.  
- **.adml files** – language-specific resource files that provide the display text for those settings in the Group Policy Management Console.  

Administrators can maintain a **Central Store** in SYSVOL where all DCs and administrators reference the same set of templates. This ensures consistency across an organisation and allows new templates (for newer Windows versions or Office apps) to be applied uniformly.

---

## Trust Types: Parent-Child, Tree-Root, External, Forest
Trusts are the mechanisms that allow authentication and resource sharing across domains and forests.

- **Parent-Child Trust**: Automatically created when a new domain is added to a tree; transitive by default.  
- **Tree-Root Trust**: Created when a new tree is added to a forest; also transitive.  
- **External Trust**: Manually created between two domains in different forests, typically non-transitive.  
- **Forest Trust**: Created between two entire forests to allow cross-forest authentication; can be one-way or two-way.  

---

## Transitive vs Non-Transitive Trusts
- **Transitive trust** means that if Domain A trusts Domain B, and Domain B trusts Domain C, then Domain A also implicitly trusts Domain C. This is common in parent-child and tree-root trusts.  
- **Non-transitive trust** means the trust is limited to the two domains explicitly involved. For example, an external trust is non-transitive — Domain A trusts Domain B, but that trust doesn’t extend beyond B.  

Understanding the difference is critical when designing cross-domain or cross-forest access strategies.

---

## Advanced AD DS – ESAE Forest
The **ESAE forest**, often called a “Red Forest”, is a **hardened, dedicated Active Directory forest** created purely for administering privileged accounts. It was introduced as a Microsoft security model to reduce the risk of credential theft (such as pass-the-hash or pass-the-ticket attacks). In this setup, privileged accounts (like Domain Admins) live in the ESAE forest and are used only on hardened admin workstations. The production forest trusts the ESAE forest, meaning admins authenticate from the secure forest when managing production AD. This reduces attack surface, enforces stronger controls, and isolates privileged access from compromised endpoints. While ESAE has largely been replaced by more modern **Privileged Access Strategy** and **Microsoft Entra Privileged Identity Management (PIM)**, it remains an important concept and is still referenced in enterprise security designs.