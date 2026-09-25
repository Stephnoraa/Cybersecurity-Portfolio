# Azure Cloud Security Foundations & Cost Governance
**Core Focus:** Cloud Security Baseline & FinOps Governance
## Tools & Technologies: 
- Azure RBAC
- Multi-Factor Authentication (MFA)
- Azure Cost Management & Billing
- Azure Policy
## Key Scope:
- Designed least-privilege RBAC architecture across 9 enterprise organizational roles (CIO, System Admin, DB Admin, Security Engineer, etc.).
- Enforced 100% MFA deployment using Conditional Access policies for admin and user sign-ins.
- Implemented Azure Cost Management controls, setting budget alerts at 80% and 100% thresholds, automated VM shutdown schedules, and cost-allocation tagging.


[Azure Security] [RBAC] [MFA] [FinOps] [Azure Cost Management] [Governance]

# Executive Summary
Rapid cloud adoption without baseline governance models introduces security exposure and unmanaged cost overruns. This project established foundational cloud security, zero-trust access boundaries, and financial controls for Maze Corporation’s Azure environment.

To uphold the Principle of Least Privilege, nine distinct organizational roles were mapped to granular Azure Role-Based Access Control (RBAC) permissions. Multi-Factor Authentication (MFA) was enforced across 100% of tenant user accounts. For fiscal governance, Azure Cost Management + Billing tools were implemented with strict monthly budget caps, multi-tier automated alerts at 80% and 100% spending thresholds, cost-allocation tagging, and automated development resource shutdown schedules, achieving a 17% reduction in cloud operating costs.
## Architecture / Threat Map
flowchart TD
'''
    subgraph Governance_and_Access_Layer [Azure Entra ID Governance Layer]
        MFA[Mandatory MFA Enforcement - 100% Adoption]
        PIM[Privileged Access & JIT Controls - Max 8h Elevation]
        RBAC[Azure RBAC Role Mapping - 9 Enterprise Roles]
    end

    subgraph Enterprise_Role_RBAC_Assignments [RBAC Permissions Structure]
        SysAdmin[System Administrator: Owner Role]
        SecEng[Cyber Security Engineer: Security Admin]
        CIO[CIO: Read-Only / Analytics Access]
        FinHead[Head of Finance: Cost Management & Billing Reader]
        DevOps[Software Engineer / DBA: Resource Specific Contributor]
    end

    subgraph FinOps_Cost_Control_Engine [Azure Cost Management + Billing]
        Budget[Monthly Budget Cap]
        Alert80[Alert Threshold 1: 80% Usage -> Email Notification]
        Alert100[Alert Threshold 2: 100% Usage -> Governance Review]
        AutoShutdown[Automated Shutdown Rules: Dev/Test VMs]
    end

    Governance_and_Access_Layer --> Enterprise_Role_RBAC_Assignments
    Enterprise_Role_RBAC_Assignments --> FinOps_Cost_Control_Engine

  '''
 
## Technical Execution
1. **Least-Privilege Role Assignment & Access Control**
Defined permissions for 9 organizational roles:
- System Administrator: Owner Role (full administrative privileges).
- Human Resource Manager: Reader / Basic Identity Management.
- Cyber Security Engineer: Security Administrator (security policies, Sentinel, monitoring).
- Chief Information Officer (CIO): Reader (read-only access to executive dashboards, audit logs, and reports).
- Intern: Restricted Contributor (isolated test resource group).
- Head of Finance: Cost Management Contributor / Billing Reader.
- Database Administrator: SQL Security Contributor / DB Administrator.
- Operations Manager: Reader / Application Performance Monitor.
- Software Engineer: Contributor (development and testing environments).
Enforced Just-In-Time (JIT) access for privileged actions, limiting elevated session durations to a maximum of 8 hours.
2. **Tenant Security Hardening & MFA Deployment**
- Enforced Azure MFA across all user accounts using conditional access policies.
- Verified authentication factors: password plus Microsoft Authenticator app / FIDO2 security keys / hardware keys for privileged accounts.
- Established break-glass emergency administrative accounts with off-site credential escrow.
3. **Cost Governance & FinOps Implementation**
- Provisioned Azure Cost Management + Billing dashboards.
- Defined monthly spend budgets with automated email alert notifications triggered at 80% and 100% budget burn rates to the Head of Finance and IT leads.
- Created mandatory tagging policies (Department, Environment, Owner) enforced via Azure Policy.
- Scheduled automated runbooks to stop non-production VMs (development/testing) outside business hours, driving a 17% overall cost optimization.
## Remediation & Lessons Learned
- Over-Permissioning Prevention: Default administrative roles expose cloud environments to lateral movement. Mapping job duties to standard custom RBAC definitions minimized the overall attack surface.
- Proactive Budgeting: Automated shutdowns and proactive threshold notifications prevent billing anomalies caused by unmonitored autoscaling or orphaned cloud resources.
