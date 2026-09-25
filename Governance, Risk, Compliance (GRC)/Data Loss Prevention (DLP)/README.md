# Data Loss Prevention (DLP) & Information Governance Engine
**Core Focus:** Data Protection Architecture
## Tools & Technologies: 
- Microsoft Purview
- Microsoft Entra ID
- DLP Engine.
## Key Scope:
- Configured DLP policies across Microsoft 365 services (Exchange, OneDrive, Teams, SharePoint).
- Defined Sensitive Information Types (SIT) using regex and custom dictionary patterns for PCI-DSS financial records, PII, and HIPAA health data.
- Enforced automatic blocking, user policy tips, admin alerts, and protective labeling.

[DLP] [Microsoft Purview] [Microsoft Entra ID] [Data Classification] [GDPR] [PCI-DSS]
# Executive Summary
Uncontrolled sharing of sensitive data across cloud collaboration platforms risks severe regulatory penalties (PCI-DSS, GDPR, HIPAA) and intellectual property leakage.

This project implemented a Data Loss Prevention (DLP) solution using Microsoft Purview and Microsoft Entra ID. The engine enforces automated detection, user policy tips, blocking, and incident logging for credit card data, healthcare records, and internal proprietary documents across Microsoft Word, PowerPoint, OneDrive, Teams, Outlook, and SharePoint environments. Fine-grained administration was established using scoped Entra ID roles.
## Architecture / Threat Map
flowchart TD

    subgraph Data_Scanning_Endpoints [Monitored Microsoft 365 Workspaces]
        M365_Docs[Word / PowerPoint Templates]
        M365_Cloud[OneDrive / SharePoint Storage]
        M365_Comms[Exchange Email / Teams Messages]
    end

    subgraph Purview_DLP_Engine [Microsoft Purview Detection Engine]
        SIT[Sensitive Info Types: PCI-DSS, PII, HIPAA, Custom Regex]
        Roles[Entra ID Scoped Roles: Compliance Administrator, DLP Auditor]
        
        SIT --> PolicyEval{Match DLP Policy?}
    end

    subgraph Protective_Actions [Automated Enforcement Actions]
        Block[Block Action & Notify User via Policy Tip]
        Alert[Trigger Admin Alert & Incident Log]
        Quarantine[Quarantine File / Auto-Apply Label]
    end

    Data_Scanning_Endpoints --> Purview_DLP_Engine
    PolicyEval -- "Yes" --> Block & Alert & Quarantine

## Technical Execution
1. Scoped RBAC Configuration in Microsoft Entra ID
- Navigated to Microsoft Purview (compliance.microsoft.com) and Azure Portal (portal.azure.com).
- Assigned solution-specific Entra ID roles rather than broad Purview role groups:
- Compliance Administrator: Full control to manage DLP policies.
- Compliance Data Administrator: Alert management and operational DLP tasks.
- Security Reader: Read-only access for security audits.
- Custom Scoped Roles: DLP Policy Reviewer, DLP Incident Responder, DLP Auditor.
2. DLP Policy Definition & Sensitive Information Types (SIT)
Defined target scanning parameters:
- Financial Data (PCI-DSS): Credit/debit card numbers validated via Luhn algorithms.
- Healthcare Data (HIPAA): Social Security Numbers, health diagnostic terms, patient IDs.
- Intellectual Property: Proprietary documentation tagged via custom regex patterns.
- Monitored channels: Exchange Online email, Teams chats, OneDrive files, SharePoint sites, Word/PowerPoint files.
  
3. Automated Enforcement Actions
Configured rule triggers:
- Block external sharing/transmission automatically.
- Render pop-up policy tips explaining the violation to end-users.
- Generate security alerts sent to the SOC with full context logs.
- Quarantine non-compliant content for administrator review, achieving a 98.1% detection success rate.
## Remediation & Lessons Learned
- Scoped Access Precision: Utilizing broad administrative roles introduces compliance risk; scoping DLP permissions through targeted Entra ID roles limits compliance data exposure.
- User Education via Policy Tips: Immediate policy tips reduce false positives by educating users at the point of action rather than relying solely on silent blocking.
