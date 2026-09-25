# Enterprise Identity & Access Management (IAM) Implementation Strategy
**Core Focus:** IAM Architecture & Lifecycle Planning
## Tools & Technologies: 
- Identity & Access Management (IAM)
- RBAC
- UAT frameworks.
## Key Scope:
Formulated a 12-week enterprise IAM deployment roadmap covering Infrastructure Setup, Integration Testing, UAT, and End-User Training.
Developed access policies, segregation of duties, and quarterly entitlement access review procedures.


[IAM] [Azure AD] [RBAC] [Access Reviews] [Zero Trust] [Least Privilege]
## Executive Summary
Unstructured account lifecycle management creates orphan accounts, privilege creep, and compliance failures. Maze Corporation required a formal Identity and Access Management (IAM) framework to govern access across hybrid cloud environments.

This project defined a 12-week IAM deployment roadmap covering directory structure alignment, role-based access rules, automated user lifecycle provisioning, quarterly access attestation reviews, and zero-trust identity guardrails. The framework eliminated excessive administrative entitlements, standardized access requests through formal business justification channels, and established 90-day automatic deprovisioning for inactive accounts.
## Architecture / Threat Map
sequenceDiagram
    autonumber
    
    actor User as Employee / Contractor
    participant Portal as Azure IAM / Entra Portal
    participant Mgr as Line Manager / Approver
    participant IAM as Identity & Access Management Engine
    participant Res as Target Resource (Azure / M365)

    User->>Portal: Request Role Access (with Business Justification)
    Portal->>Mgr: Route Approval Notification
    Mgr-->>Portal: Approve Access Request
    Portal->>IAM: Provision Least-Privilege RBAC Assignment
    IAM->>Res: Grant Access (Time-Bound / JIT)
    Note over IAM,Res: Quarterly Access Review Cycle (90 Days Inactivity Auto-Revoke)
    IAM->>Mgr: Attestation Report (Verify Continued Need)
    Mgr-->>IAM: Confirm or Revoke Permissions
    
## Technical Execution
1. **Identity Structure & Role Definitions**
- Structured directory organizational units (OUs) and Azure AD security groups aligned with corporate business functions.
- Implemented Role-Based Access Control (RBAC) following NIST SP 800-53 and AWS/Azure security benchmarks.
2. **User Lifecycle & Provisioning Workflows**
- Configured standardized access request workflows requiring formal system owner and manager approval prior to permission granting.
- Enforced automated account disabling and permission revocation for accounts inactive for 90 days or longer.
3. **Access Governance & Entitlement Reviews**
- Formulated a quarterly access review schedule requiring department heads to attest to employee access rights.
- Integrated Separation of Duties (SoD) policies preventing single users from holding conflicting administrative privileges (e.g., security auditor and policy editor).
## Remediation & Lessons Learned
- Attestation Enforcement: Access creep occurs rapidly without mandatory periodic reviews. Implementing quarterly manager attestation ensures legacy permissions are systematically revoked.
- JIT Elevation: Transitioning permanent administrative accounts to Just-In-Time (JIT) access reduces the operational window for credential exploitation.
