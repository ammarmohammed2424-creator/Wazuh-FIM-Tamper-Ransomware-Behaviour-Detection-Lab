# Wazuh-FIM-Tamper-Detection-Lab
Real-time file integrity monitoring (FIM) on a Windows endpoint using Wazuh 4.7.5, with who-data attribution, before/after content diffs, and custom detection rules mapped to MITRE ATT&amp;CK.

<img width="856" height="104" alt="Screenshot 2026-10-06 112445" src="https://github.com/user-attachments/assets/638ab278-9a72-432f-8a42-d6d2845e7c98" />

1. Screenshot 2026-10-06 112445_3.pngStage: Endpoint Integrity Configuration (ossec.conf)   Technical Action: Modifying the Wazuh Agent’s local configuration file to define active monitoring policies for target directories.


2.Key Details & Parameters:Directory Targeting: Configures the Syscheck module to monitor sensitive user locations (C:\Users\<USER>\Downloads).   whodata="yes": Replaces traditional periodic polling with real-time Windows Audit Policies (SACLs) to capture deep telemetry, including process IDs, process paths, and user account SIDs.   report_changes="yes": Enables content diff tracking, allowing Wazuh to store and display exact file modifications before and after an edit.


<img width="1768" height="896" alt="Screenshot 2026-10-06 112305" src="https://github.com/user-attachments/assets/b014f6cc-8020-48e9-819b-cf0a6135b4ff" />


Stage: Agent Service Synchronization & Log Validation   Technical Action: Verifying agent startup and module initialization via PowerShell using Get-Content "C:\Program Files (x86)\ossec-agent\ossec.log" -Tail 30.   


Key Details & Telemetry:System Readiness: Confirms core agent processes (pid: 3316) started without XML syntax errors or rule failures.   Active Scans: Shows active initialization of Security Configuration Assessment (cis_win11_enterprise.yml), Syscollector, and Rootcheck routines.   

FIM Real-Time Engine: Captures the critical confirmation logs validating that real-time monitoring and Whodata integration are active:   

INFO: (6012): Real-time file integrity monitoring started.   

INFO: (6019): File integrity monitoring real-time Whodata engine started.


<img width="1793" height="798" alt="Screenshot 2026-10-06 112133" src="https://github.com/user-attachments/assets/f4afd219-53ef-443b-ba80-38a47221f26b" />


Stage: Threat Telemetry Analysis & Incident Dashboard   Technical Action: Examining the ingested FIM alert in the Wazuh Security Events dashboard after executing a file modification test.   

Key Details & Forensic Data:Process Attribution: Identifies the exact application responsible for the file change (syscheck.audit.process.name: Notepad.exe) along with its Process ID (14084).   

User Context: Displays the specific local account (syscheck.audit.user.name) and unique Windows SID (syscheck.audit.user.id) that initiated the process.   

File Integrity Metrics: Captures cryptographic hash changes across multiple algorithms (md5_before vs. md5_after, SHA1, SHA256) and logs attribute updates (size, mtime).   

Content Delta (syscheck.diff): Highlights exact line-by-line file content modifications (< xyz to > x), demonstrating ransomware-like file tampering detection. 
