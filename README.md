# SOC-Home-Lab
SOC home lab built using Kali Linux, Windows, Sysmon, and Splunk to generate and analyze endpoint telemetry.

## Overview
This project is a home lab using Kali Linux, Windows, Sysmon, and Splunk to simulate analysis of endpoint telemetry.

## Objective
The goal of this lab is to generate suspicious activity in a virtual environment using a Kali Machine then investigate the resulting logs in Splunk (and using Sysmon) in a Windows Machine.

## Lab Environment
- Kali Linux VM
- Windows VM
- VirtualBox NAT Network
- Sysmon
- Splunk
- Metasploit multi/handler

## Activity Simulated
- Nmap scan against the Windows VM
- Malware payload execution (built using msfvenom)
- Reverse TCP connection from Windows to Kali using msfconsole as the handler.
- Command shell activity
- Discovery commands such as net user, net localgroup, and ipconfig
- Ingestion of these logs using Sysmon into Splunk for analysis and investigation.

## Key Findings
- Nmap identified port 3389 as open in the Windows Machine, indicating RDP (Remote Desktop Protocol) is available.
- Investigation using Splunk showed that the parent process which is the malware downloaded from the HTTP server (made using Python within the Kali VM) spawned a child process, cmd.exe which then performed discovery commands (net user, net localgroup, ipconfig)

## Sample SPL Queries

### Search endpoint logs
```spl
index=endpoint
index=endpoint document.pdf.exe
index=endpoint {a20d5ff3-7490-6a3a-3130-000000000a00}
| table _time,ParentImage,CommandLine
