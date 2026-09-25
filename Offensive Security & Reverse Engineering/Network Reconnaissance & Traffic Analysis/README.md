# Network Reconnaissance, Traffic Analysis & Cryptanalysis
**Core Focus:** Network & Artifact Exploitation
## Tools & Technologies: 
- Nmap
- Wireshark
- John the Ripper
- VeraCrypt
- PE Explorer.
## Key Scope:
- Conducted full 65,535-port Nmap service scans identifying exposed FTP (Port 21) and HTTP (Port 80) services.
- Sniffed unencrypted HTTP POST traffic in Wireshark to intercept plaintext credentials.
- Decrypted VeraCrypt volumes by cracking MD5 password hashes with John the Ripper (rockyou.txt).
- Extracted PE binary entry point headers (0x004237B0 / 0x0001F3A0) using PE Explorer disassembler view.


[Nmap] [Wireshark] [John the Ripper] [VeraCrypt] [PE Explorer] [Cryptanalysis] [Reverse Engineering]
# Executive Summary
Offensive security operations require proficiency in network scanning, unencrypted traffic sniffing, cryptanalysis, and binary structure analysis. This lab exercise evaluated a target network through comprehensive port scanning, credential capture, hash cracking, and reverse engineering.

The exercise identified exposed network services using Nmap, captured unencrypted HTTP login credentials via Wireshark packet analysis, cracked an MD5 password hash using John the Ripper to decrypt a VeraCrypt container, and extracted binary execution headers using PE Explorer.
Architecture / Threat Map
flowchart TD

    subgraph Network_Recon [Network Scanning & Sniffing]
        Nmap[Nmap 65k Port Scan] --> Port80[Port 80 HTTP & Port 21 FTP]
        Wireshark[Wireshark Packet Capture] -- "Filter: http.request.method == POST" --> Cleartext[Captured Plaintext Credentials]
    end

    subgraph Cryptanalysis_Module [Hash Cracking & Volume Decryption]
        HashFile[Hash in encoded.txt: 482c811da5d5b4bc6d497ffa98491e38]
        John[John the Ripper + rockyou.txt]
        DecryptedPass[Cracked Password: password123]
        VeraCrypt[VeraCrypt Container: veracrypt.txt]
        SecretCode[Secret Code Revealed: 'never giveup']

        HashFile --> John --> DecryptedPass
        DecryptedPass & VeraCrypt --> SecretCode
    end

    subgraph Binary_Analysis [Reverse Engineering]
        PEFile[Binary: pe.explorer_setup.exe] --> PEExp[PE Explorer Header View]
        PEExp --> EntryPoint[Extracted Entry Point: 0x004237B0 / 0x0001F3A0]
    end

## Technical Execution
1. **Network Port Scanning & Traffic Sniffing**
- Executed full 65,535-port Nmap scan:
nmap -p- -sV testphp.vulnweb.com
Identified open Port 21 (FTP) and Port 80 (HTTP running nginx 1.19.0).
- Started Wireshark capture during a target web login attempt. Applied display filter:

http.request.method == "POST"

Extracted cleartext login credentials transmitted over unencrypted HTTP.
2. **Cryptanalysis & VeraCrypt Container Decryption**
- Extracted MD5 hash from encoded.txt: 482c811da5d5b4bc6d497ffa98491e38.
- Executed hash identification and cracking using John the Ripper on Kali Linux:
echo "482c811da5d5b4bc6d497ffa98491e38" > hash.txt
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
**Cracked Plaintext Password:** password123
- Mounted encrypted volume veracrypt.txt in VeraCrypt using password123. Unlocked virtual drive X: and retrieved secret file content: "The secret code is :- never giveup".
3. **Executable Binary Analysis with PE Explorer**
- Loaded pe.explorer_setup.exe into PE Explorer.
- Navigated to PE Header details and Disassembler view.
- Extracted program Entry Point address: 0x004237B0 (also verified as 0x0001F3A0 depending on header relocation), defining the exact memory location where binary execution begins.

## Remediation & Lessons Learned
- Enforce Transport Layer Security: Transmitting credentials over HTTP exposes users to simple packet sniffing attacks; mandatory HTTPS redirection is critical.
- Password Complexity Standards: Weak passwords hashed with outdated algorithms (MD5) can be cracked rapidly using wordlists; strong password policies and modern key derivation functions (Argon2, bcrypt) are required.
