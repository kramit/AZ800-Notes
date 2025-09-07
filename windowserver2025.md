## New Features in Windows Server 2025

### Desktop & Usability
- **In-place upgrade from Windows Server 2012 R2 onward** — allows skipping multiple versions in one step  
- **Windows 11-style desktop shell** on first sign-in  
- **Bluetooth support** — for mice, keyboards, headsets, audio devices, etc.  
- **Windows Terminal installed by default** — modern multi-shell interface  
- **WinGet installed by default** — command-line package manager  
- **Feedback Hub** included for direct feedback  

### Monitoring & Diagnostics
- **DTrace built-in** — real-time performance tracing of system and user-space  

### Hybrid & Licensing
- **Deep Azure Arc integration** — includes centralized management, hybrid control, and pay-as-you-go licensing  

### Security Enhancements
- **Hotpatching (preview)** — apply security updates without restarting (via Azure Arc)  
- **Credential Guard enabled by default** on supported systems  
- **Enhanced Active Directory**:  
  - Optional **32 kB database page size** for scalability  
  - **LDAP encryption**, **TLS 1.3**, **stronger machine account passwords**, **confidential attribute protection**, and more  
- **SMB Security Upgrades**:  
  - **SMB over QUIC** support in all editions, not just Azure  
  - **Mandatory SMB signing**, **authentication rate limiting**, **NTLM blocking**, and encrypted connections by default  
- **Managed Service Account (dMSA)** support — simplified credential management  
- **OpenSSH pre-installed** — secure, modern remote management  
- **LAPS enhancements** — safer local admin password management with passphrases, rollback detection, and process termination features  
- **Virtualization-based Security (VBS)** support — enclaves and key protection  

### Virtualization & Hyper-V
- **Hyper-V scalability**: support up to **4 PB RAM** and **2,048 logical processors per host**, and up to **240 TB RAM + 2,048 vCPUs per VM**  
- **GPU partitioning** — share GPUs across VMs  
- **Hypervisor-enforced paging translation (HVPT)** — boosts memory translation protection  
- **Generation 2 VMs** now default in Hyper-V Manager  
- **Workgroup clusters** supported — clustering without AD domain  
- **AccelNet** — simplified SR-IOV (network virtualization) setup  

### Storage & File Systems
- **NVMe performance improvements** — up to ~60 % higher IOPS vs Server 2022  
- **Block cloning with Dev Drive (ReFS)** — accelerates file operations  
- **Native deduplication & compression** on ReFS — improves storage efficiency  
- **Storage Replica compression** — reduces network usage during replication  

### Containers & Developer Workflows
- **Improved container portability** across hosts & environments  
- **OpenSSH and Windows Terminal** — built-in tools for modern DevOps workflows  

### Monitoring & Telemetry
- **DTrace + Azure Monitor via Azure Arc** — unified, real-time observability  


## Why Upgrade? Key Benefits

- **Seamless management**: Azure Arc delivers unified hybrid control and flexible pay-as-you-go licensing.  
- **Extremely low downtime**: Hotpatching enables applying updates without rebooting.  
- **Boosted security**: From Credential Guard enabled by default, encrypted LDAP/SMB traffic, to retirement of legacy insecure protocols.  
- **Scalable virtualization**: Support for massive RAM and vCPU counts, plus GPU sharing—ready for AI, databases, and high-performance VMs.  
- **Superior storage performance**: NVMe tuning, block cloning, and efficient ReFS features help reduce costs and improve throughput.  
- **Modern administration practices**: Built-in command-line tools (WinGet, Terminal, OpenSSH), better container support, and advanced monitoring ease automation.  
- **Future-ready architecture**: Cleaner UI, better hybrid integration, and enterprise-ready stack designed for evolving workloads.  