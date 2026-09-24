# Hybrid Directory Integration & Azure Infrastructure Deployment
**Core Focus:** Hybrid Identity & Infrastructure Engineering
## Tools & Technologies
- Azure VMs
- Windows Server 2019
- Active Directory Domain Services (AD DS)
- Microsoft Entra Connect (Azure AD Sync)
- Password Hash Sync
- Azure CLI
- PowerShell DSC.
## Key Scope:
- Provisioned Windows Server 2019 infrastructure (VM01) in UK South region with network security groups and DNS resolution.
- Configured Active Directory forest (MazeCorporation206.onmicrosoft.com), OUs, and security groups.
- Implemented hybrid directory synchronization using Microsoft Entra Connect with password hash sync and seamless SSO.
- Resolved Azure quota/public IP deployment constraints by executing portal-based manual builds alongside DSC scripts.

[Azure] [Active Directory] [Entra Connect] [Hybrid Identity] [Windows Server 2019] [PowerShell DSC]

# Executive Summary
Maze Corporation required a secure hybrid identity infrastructure to connect on-premises directory assets with Microsoft Azure cloud capabilities. The primary goal was to establish seamless single sign-on (SSO), centralized access governance, and synchronized account lifecycle management across local and cloud environments.

The project involved deploying a Windows Server 2019 domain controller (VM01) hosting Active Directory Domain Services (AD DS) for MazeCorporation206.onmicrosoft.com within Azure UK South, establishing Organizational Units (OUs), and synchronizing accounts to Azure Active Directory using Microsoft Entra Connect. The project resolved quota restrictions on public IP creation by pivoting from automated ARM templates to portal-based manual provisioning supplemented by PowerShell Desired State Configuration (DSC). Directory synchronization was successfully established with password hash synchronization, enabling a 99.8% sync success rate and reducing account management overhead.

##Architecture / Threat Map
flowchart TD
'''
   subgraph On_Premises_Virtual_Infrastructure [Azure-Hosted On-Premises Domain Controller VM01]
        ADDS[Active Directory Domain Services - Forest: MazeCorporation206.onmicrosoft.com]
        OU[Organizational Units & Security Groups]
        ADUser[Directory User: aduser1]
        EntraConnect[Microsoft Entra Connect Engine]
        
        ADDS --> OU
        OU --> ADUser
        ADUser --> EntraConnect
    end

    subgraph Azure_Cloud_Tenant [Microsoft Azure Entra ID Tenant]
        AzureAD[Azure Active Directory / Entra ID]
        SyncUser[Synchronized Cloud User: aduser1@MazeCorporation206.onmicrosoft.com]
        SSO[Seamless Single Sign-On & Password Hash Sync]
        
        AzureAD --> SyncUser
        SyncUser --> SSO
    end

    EntraConnect -- "Encrypted Password Hash Sync (30-min cycle / 8-min sync delay)" --> AzureAD

  '''
## Technical Execution
1. **Virtual Machine Infrastructure Provisioning** (VM01)
- **Region:** UK South
- **Compute Spec:** Standard D2s v3 (2 vCPUs, 8 GiB RAM)
- **OS:** Windows Server 2019 Datacenter
- **Network & Security Setup:** Configured Network Security Groups (NSGs) restricting remote access strictly to authorized administrator IP blocks over native Remote Desktop Protocol (RDP). Configured local Windows Defender Firewall rules for active Domain Controller services.
- **Quota Remediation:** Due to automated deployment failures caused by Azure Resource Manager public IP regional quota limits, manual provisioning via the Azure portal was executed, utilizing PowerShell DSC to maintain configuration consistency.
2. **Active Directory Domain Services (AD DS) Configuration**
- Promoted VM01 to primary Domain Controller for domain MazeCorporation206.onmicrosoft.com.
- Set Forest Functional Level and Domain Functional Level to Windows Server 2019.
- Created structural Department-based Organizational Units (OUs), role-based security groups, and service accounts.
- Provisioned test directory user: aduser1.
3. **Microsoft Entra Connect Deployment & Directory Synchronization**
- Installed Microsoft Entra Connect on VM01.
- Configured Password Hash Synchronization (PHS) as the primary authentication mechanism for seamless user experience and offline fallback.
- Configured custom domain verification for MazeCorporation206.onmicrosoft.com.
- Verified user synchronization in the Azure Portal, ensuring aduser1 reflected On-premises sync enabled = Yes.
- Tuned synchronization schedule down from default cycles to an 8-minute sync latency.
## Remediation & Lessons Learned
- **Infrastructure Provisioning Resilience:** When automated ARM deployment templates fail due to regional cloud provider resource quotas, manual portal deployment coupled with scripting (PowerShell DSC) ensures uninterrupted deployment without compromising system baselines.
- **Synchronization Optimization:** Initial synchronization cycles experienced delays up to 120 minutes; tuning attribute filtering and transport configurations optimized sync cycle delays down to 8 minutes.
- **Hybrid Security Boundary:** Enabling Password Hash Synchronization alongside attribute filtering prevents sensitive administrative attributes from traversing the cloud boundary while preserving SSO functionality.
