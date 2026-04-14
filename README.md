# TryHackMe – VulnNet: Active  
## Active Directory Exploitation and SYSTEM Compromise  

---
 
## Medium Blog  

A detailed explanation of this lab, including methodology and reasoning, is available on Medium:

https://medium.com/@tusharmumbre/tryhackme-medium-windows-ad-writeup-71981f81da02  

---

## 1. Introduction  

This repository presents a detailed penetration testing walkthrough of the **VulnNet: Active** room on TryHackMe.  

The objective of this lab was to simulate a real-world attack against a Windows Active Directory environment and achieve full SYSTEM-level compromise starting from an unauthenticated position.  

---

## 2. Target Information  

**Platform:** TryHackMe  
**Room:** VulnNet: Active  
**Difficulty:** Medium  
**Operating System:** Windows (Active Directory Environment)  

---

## 3. Methodology  

The attack was performed following a structured penetration testing approach:  

- Reconnaissance  
- Enumeration  
- Exploitation  
- Post-Exploitation  
- Privilege Escalation  

---

## 4. Attack Overview  

The compromise was achieved through multiple vulnerabilities chained together:  

**Attack Chain:**  
Unauthenticated Redis → File Read → NTLM Capture → Password Cracking → SMB Access → Scheduled Task Abuse → PrintNightmare → SYSTEM Access  

---

## 5. Detailed Execution  

### 5.1 Reconnaissance  
A full port scan using Nmap identified open services, including an exposed Redis service running on port 6379.  

### 5.2 Redis Enumeration  
The Redis instance was accessible without authentication. Enumeration revealed system paths that exposed a valid Windows username:  
`enterprise-security`  

### 5.3 User Flag via Lua Exploit  
Using Redis Lua scripting, local file access was achieved. The `user.txt` flag was retrieved through an error-based file read technique.  

### 5.4 NTLM Hash Capture  
A UNC path was used to force the target system to authenticate to the attack machine. Responder captured the NTLMv2 hash of the user.  

### 5.5 Password Cracking  
The captured hash was cracked using Hashcat with a wordlist. Weak password policy enabled quick recovery of credentials.  

### 5.6 SMB Enumeration and Access  
Using valid credentials, SMB shares were accessed. A scheduled PowerShell script was identified and replaced with a malicious reverse shell payload.  

### 5.7 Privilege Escalation (PrintNightmare)  
The PrintNightmare vulnerability (CVE-2021-1675) was exploited to escalate privileges. A new administrative user was created and added to privileged groups.  

### 5.8 SYSTEM Access  
Using administrative credentials, a SYSTEM-level shell was obtained. The final flag was retrieved from the Administrator desktop.  

---

## 6. Key Findings  

- Unauthenticated Redis instance exposed sensitive data  
- Redis Lua scripting enabled arbitrary file read  
- NTLM authentication allowed credential capture  
- Weak password policy enabled rapid cracking  
- Scheduled task abuse allowed initial access  
- PrintNightmare vulnerability enabled privilege escalation  

---

## 7. Security Recommendations  

- Secure Redis with authentication and restrict access  
- Disable unnecessary services exposed to the network  
- Block outbound SMB traffic to prevent NTLM capture  
- Enforce strong password policies  
- Apply security patches (especially for PrintNightmare)  
- Disable Print Spooler where not required  

---

## 8. Tools Utilized  

- Nmap  
- Redis CLI  
- Responder  
- Hashcat  
- CrackMapExec  
- SMBClient  
- Metasploit  
- Impacket (psexec)  

---

## 9. Conclusion  

This lab demonstrates how multiple vulnerabilities, when chained together, can lead to complete system compromise.  

It highlights the importance of securing exposed services, enforcing strong authentication mechanisms, and applying timely patches in enterprise environments.  

---

## 10. Detailed Report  

The complete penetration testing report is available below:  

- [Download Full Report](VulnNet-Active-Report.docx)  

This document contains full commands, outputs, screenshots, and detailed technical explanations.  

---

## 11. Purpose  

This project is part of my cybersecurity learning journey and demonstrates practical knowledge of real-world attack techniques, Active Directory exploitation, and privilege escalation methodologies.  
