# Enterprise SIEM Architecture & Threat Intelligence Ingestion
**Core Focus:** Cloud SIEM Implementation
## Tools & Technologies: 
- Azure Sentinel
- KQL (Kusto Query Language)
- Log Analytics Workspace, Threat Intelligence Feeds.
## Key Scope:
- Provisioned Azure Sentinel (Team 8 Workspace) ingesting ~15GB of log telemetry daily across 15+ connected sources.
- Configured custom indicator tagging for threat feeds (IPs, URLs, domain hashes) from Microsoft Defender Threat Intelligence.
- Established baseline system health monitoring (CPU, network throughput, authentication frequency)

[Azure Sentinel] [SIEM] [KQL] [Threat Intelligence] [Log Analytics] [Cybersecurity]

## Executive Summary
Maze Corporation lacked centralized security event visibility across its hybrid cloud environment, hindering early threat detection. This project implemented Microsoft Sentinel as a cloud-native Security Information and Event Management (SIEM) solution to aggregate, correlate, and analyze security telemetry across infrastructure assets.

The deployment established an Azure Log Analytics workspace (Team 8 Workspace in Microsoft Sentinel / Team8-MazeCorp-Sentinel) ingesting approximately 15 GB of daily log data across 15+ connected sources, including Windows Domain Controllers, Azure Sentinel agents, firewall logs, and Entra ID sign-in events. Custom Threat Intelligence (TI) indicators (IPs, URLs, domain hashes) were integrated from Microsoft Defender Threat Intelligence, and Log Reader permissions were explicitly configured to maintain log integrity.
## Architecture / Threat Map
flowchart LR

        subgraph Data_Sources [Telemetry & Log Ingestion Sources]
        DC_Logs[Windows DC / VM Telemetry]
        Entra_Logs[Entra ID Audit & Sign-In Logs]
        Net_Logs[Firewall & NSG Network Traffic]
        TI_Feeds[Microsoft Defender Threat Intelligence]
    end

    subgraph Sentinel_SIEM_Core [Azure Sentinel Workspace: Team8-MazeCorp-Sentinel]
        Ingest[Log Analytics Ingestion Engine (~15 GB/day)]
        Analytics[Custom KQL Analytics Rules & Analytics Engine]
        TI_Map[Indicator Tagging & Threat Mapping]
        
        Ingest --> Analytics
        TI_Feeds --> TI_Map
        TI_Map --> Analytics
    end

    subgraph SOC_Operations [Security Operations Center]
        RBAC_Logs[Log Reader Role Access: Akinola Adiyan & Benjamin Dariya]
        Alerts[Security Incident Generation & Triage]
    end

    Data_Sources --> Ingest
    Analytics --> Alerts
    Alerts --> RBAC_Logs
    
## Technical Execution
1. **Log Analytics & Sentinel Provisioning**
- Provisioned a centralized Log Analytics Workspace: Team 8 Workspace in Microsoft Sentinel.
- Provisioned a supporting log collector VM specified with 4 vCPUs, 16 GB RAM, and 128 GB SSD to process log throughput without bottlenecking.
- Connected 15+ enterprise data sources, achieving ~15 GB daily log ingestion.
2. **Access Control & Log Security**
- Configured granular Log Reader permissions assigned specifically to designated security analysts (Akinola Adiyan and Benjamin Dariya) to restrict unauthorized access to raw security logs.
- Documented baseline operational metrics over a 7-day monitoring period:
- Average CPU Utilization: 15–25%
- Memory Usage: 40–60%
- Network Throughput: 100–500 MB/hour
- Daily Authentication Frequency: 8–12 sessions per user/day.
3. **Threat Intelligence Integration & Indicator Tagging**
- Integrated threat intelligence feeds from Microsoft Defender Threat Intelligence.
- Created indicator tags categorizing Indicators of Compromise (IoCs) by actor type, malware family, and severity.
- Developed custom PowerShell scripts to automate bulk IoC imports into Sentinel lookup tables.

## Remediation & Lessons Learned
- Log Ingestion Sizing: Estimating capacity prior to deployment prevents log drop scenarios. Allocating dedicated VM specs (4 vCPUs, 16 GB RAM) ensured continuous ingestion of 15 GB/day.
- TI Contextualization: Raw IoC feeds create noise; applying custom indicator tagging enables higher-fidelity correlation with internal telemetry.
