# Enterprise Incident Response Playbook Engineering
**Core Focus:** IR Process Development
##Tools & Technologies: 
- NIST SP 800-61r2
- Incident Response Frameworks
-  Markdown/Draw.io.
## Key Scope:
- Built standardized, decision-tree driven IR playbooks for Malware Outbreaks, Suspicious Sign-Ins, and Email-Borne Threats.
- Structured containment procedures, forensic preservation guidelines, communication matrices, and post-incident review metrics (MTTD/MTTC).

[Incident Response] [NIST SP 800-61r2] [Playbooks] [SOC Operations] [Security Governance]

# Executive Summary
Uncoordinated incident handling leads to inconsistent containment, lost forensic evidence, and prolonged system outages. To standardize response workflows, this project established NIST SP 800-61r2 aligned Incident Response (IR) Playbooks for Maze Corporation.

The engineering effort resulted in structured, decision-tree driven playbooks addressing three critical threat profiles: Malware Outbreaks, Suspicious Login / Identity Anomalies, and Email-Borne Phishing Threats. These playbooks define step-by-step containment procedures, forensic preservation protocols, communication escalation matrices, and metric tracking mechanisms targeting a Mean Time to Detect (MTTD) of under 8 minutes and Mean Time to Respond (MTTR) under 18 minutes.
## Architecture / Threat Map
flowchart TD
  subgraph Incident_Lifecycle_NIST_800_61 [NIST SP 800-61r2 Incident Response Flow]
        
        Detect[1. Detection & Analysis - SIEM Alerts / User Reports]
        Contain[2. Containment - Network Isolation & Token Revocation]
        Eradicate[3. Eradication - Malware Removal & Registry Cleaning]
        Recover[4. Recovery - System Hardening & Monitoring]
        Lessons[5. Post-Incident Activity - Lessons Learned & Playbook Tuning]
        
        Detect --> Contain --> Eradicate --> Recover --> Lessons
    end

    subgraph Playbook_Execution_Types [Engineered Incident Playbooks]
        PB_Malware[Malware Outbreak Playbook]
        PB_Identity[Suspicious Sign-In / Account Compromise Playbook]
        PB_Phish[Phishing & Email Threat Playbook]
    end

    Detect --> PB_Malware & PB_Identity & PB_Phish

## Technical Execution
1. **Malware Outbreak Playbook Architecture**
- Preparation: Pre-stage isolated networks, deploy Endpoint Detection and Response (EDR) agents, maintain forensic imaging tools.
- Detection & Analysis: Trigger alerts on high-confidence file hashes (e.g., WannaCry) or C2 network traffic (AsyncRAT). Validate alert fidelity.
- Containment: Automatically disconnect affected endpoints from the local network via EDR API; block malicious C2 IP addresses at the perimeter firewall.
- Eradication: Terminate malicious processes, delete registry persistence keys (schtasks), and remove payload artifacts.
- Recovery: Restore endpoints from validated clean backups, re-apply security baseline configurations, monitor endpoint for 72 hours post-remediation.
2. **Suspicious Login / Compromised Account Playbook**
- Detection: Trigger alerts on impossible travel, anomalous geographic access, or repeated failed logins (e.g., Erwin Smith incident).
- Containment: Immediately revoke active Entra ID refresh tokens, lock user account, reset credentials, and require MFA re-enrollment.
- Analysis: Review sign-in logs to identify lateral movement or exfiltration attempts.
3. **Metrics & Escalation Framework**
- Defined clear notification paths for VVIPs and executive leadership.
- Implemented metric tracking: Target MTTD < 10 minutes (achieved 8 min); Target MTTR < 30 minutes (achieved 18 min).

## Remediation & Lessons Learned
- Standardized Operations: Documented decision trees prevent analyst error during high-stress incident scenarios.
- Continuous Playbook Testing: Conducting periodic tabletop exercises ensures playbooks adapt to changing adversary tactics, techniques, and procedures (TTPs).
