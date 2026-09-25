# Cybersecurity Risk Register & Matrix Assessment
**Core Focus:** Qualitative Risk Analysis
## Tools & Technologies: 
- Risk Matrix Scoring Frameworks ($Likelihood \times Impact$).
## Key Scope:
- Created a qualitative risk register evaluating business email compromise, user database exposure, unencrypted backups, and physical theft.
- Calculated priority scores to guide mitigation investments.

[Risk Management] [Risk Register] [NIST SP 800-30] [Risk Matrix] [Risk Mitigation]
# Executive Summary
Without qualitative and quantitative risk tracking, organizations struggle to allocate security resources effectively. This project established a formal Risk Register and Assessment Framework for Maze Corporation to identify, analyze, and mitigate cyber risks.

Using a standardized Risk Matrix scoring system ($Likelihood \times Impact$), the assessment evaluated primary threat scenarios including Business Email Compromise (BEC), user database exposure, unencrypted backups, and physical device theft. Mitigation controls were mapped to reduce residual risk levels to acceptable thresholds.
## Architecture / Threat Map
quadrantChart

    title Cybersecurity Risk Assessment Matrix
    x-axis Low Likelihood --> High Likelihood
    y-axis Low Impact --> High Impact
    quadrant-1 Immediate Action / Critical
    quadrant-2 High Priority
    quadrant-3 Low Priority
    quadrant-4 Medium Priority
    "User Database Exposure": [0.35, 0.90]
    "Business Email Compromise (BEC)": [0.85, 0.85]
    "Unencrypted Backup Exfiltration": [0.40, 0.75]
    "Physical Device Theft": [0.65, 0.35]
## Technical Execution
1. Risk Register Development & Scoring Methodology
- Established a 5x5 Risk Matrix scoring engine evaluating Likelihood (1–5) and Impact (1–5) to compute Overall Risk Score ($Risk = L \times I$).
2. Threat Scenario Analysis & Categorization
- Scenario 1: Business Email Compromise (BEC) / Phishing
Initial Risk: Likelihood = 5, Impact = 4 (Score: 20 - Critical).
Controls: Advanced Email Security Gateway, Mandatory MFA, Security Awareness Training.
Residual Risk Score: 6 (Low).
- Scenario 2: User Database Exposure / SQL Injection
Initial Risk: Likelihood = 3, Impact = 5 (Score: 15 - High).
Controls: Prepared statements, Web Application Firewall (WAF), database encryption at rest.
Residual Risk Score: 4 (Low).
- Scenario 3: Unencrypted Backup Exfiltration
Initial Risk: Likelihood = 2, Impact = 4 (Score: 8 - Medium).
Controls: AES-256 backup encryption, access key rotation.
Residual Risk Score: 2 (Low).
- Scenario 4: Physical Laptop Theft
Initial Risk: Likelihood = 4, Impact = 2 (Score: 8 - Medium).
Controls: BitLocker full-disk encryption, remote wipe via MDM.
Residual Risk Score: 2 (Low).
3. Risk Treatment & Reporting
- Formulated formal risk treatment plans (Avoid, Transfer, Mitigate, Accept).
- Integrated quarterly risk reporting cycles presented to the executive leadership team.
## Remediation & Lessons Learned
- Residual Risk Verification: Mitigations must be validated after control implementation; measuring residual risk ensures implemented controls operate effectively.
- Continuous Assessment: Risk registers must be treated as dynamic documents updated following major technical changes or security incidents.
