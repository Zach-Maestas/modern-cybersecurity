# Lab 1 – Service Enumeration & Discovery (FTP / SMB / HTTP)
**Course:** CS 456 – Modern Cybersecurity  
**Repo path:** `modern-cybersecurity/homeworks/hw5`

---

## 📌 Lab objective
Enumerate services on the target host (`10.89.0.6`) and document findings for **FTP**, **SMB**, and **HTTP**. Demonstrate logins where allowed, identify weak credentials or misconfigurations, capture evidence (screenshots and Nmap outputs), and provide remediation recommendations.

All screenshots referenced below are expected to be stored in this folder’s `images/` subdirectory and raw Nmap outputs in `scans/` (e.g., `scans/scan1_sv_sc.txt`, `scans/scan2_ftp.txt`, `scans/scan3_smb.txt`, `scans/scan4_http.txt`).

---

## 🖼️ Embedded Evidence (screenshots)
**Anonymous / credentialed FTP login (evidence):**  
![breached FTP login screenshot](images/breached_ftp.png)

**Target interface / IP output:**  
![ip a output screenshot](images/ip_adr.png)

**Target web root / index page (HTTP):**  
![metasploitable web page screenshot](images/metasploitable_webpage.png)

---

## 🛠 Commands run (examples)
```bash
# 1) General service discovery + default NSE scripts
nmap -sV -sC -oN scans/scan1_sv_sc.txt 10.89.0.6

# 2) FTP focused (anonymous + brute scripts)
nmap -p 21 --script ftp-anon,ftp-brute -oN scans/scan2_ftp.txt 10.89.0.6

# 3) SMB enumeration (shares & users)
nmap -p 445 --script smb-enum-shares,smb-enum-users -oN scans/scan3_smb.txt 10.89.0.6

# 4) HTTP enumeration (common paths)
nmap -p 80,8180 --script http-enum -oN scans/scan4_http.txt 10.89.0.6
```

--- 

## Raw outputs saved in `scans/`
- `scans/scan1_sv_sc.txt` (service/version + default scripts)  
- `scans/scan2_ftp.txt` (ftp-anon, ftp-brute results)  
- `scans/scan3_smb.txt` (smb-enum-shares, smb-enum-users results)  
- `scans/scan4_http.txt` (http-enum results)

---

## 🔎 Discoveries & Findings

### 1) Anonymous / weak FTP access
- **What I found:** The FTP server allows **anonymous** logins and also accepts trivial credentials (e.g., `user:user`). Service banner indicates `vsftpd 2.3.4`.
- **Why it’s bad:** Anonymous FTP and weak credentials let unauthenticated or trivially authenticated users read, download, and potentially upload files. This can lead to data exposure, defacement, or placing backdoors/web shells on the host.
- **Evidence:** `scans/scan1_sv_sc.txt`, `scans/scan2_ftp.txt` and `images/breached_ftp.png`.

### 2) SMB shares & IPC abuse
- **What I found:** SMB (port 445) exposes multiple shares. `IPC$` and `tmp` show anonymous READ/WRITE or weak access in the lab. `smb-enum-users` enumerated multiple accounts (many disabled, some active).
- **Why it’s bad:** Writable SMB shares allow attackers to drop files/scripts or execute lateral-movement techniques. `IPC$` is an RPC-like endpoint used for inter-process communication; abuse can enable credential enumeration, RPC-based attacks, or remote service interactions (e.g., pass-the-hash).
- **Evidence:** `scans/scan3_smb.txt`.

### 3) HTTP apps & potentially vulnerable directories
- **What I found:** HTTP (port 80) serves the Metasploitable index and exposes multiple web apps (phpMyAdmin, DVWA, WebDAV, TikiWiki, Tomcat manager on alt ports). Services are outdated (Apache 2.2.x, Tomcat 5.5).
- **Why it’s bad:** Unpatched web apps and admin panels are frequent exploit vectors (RCE, SQLi, LFI/RFI, file upload). Public admin interfaces increase risk of brute-force or exploitation.
- **Evidence:** `scans/scan4_http.txt` and `images/metasploitable_webpage.png`.

---

## 🔐 Impact & Risk Summary
- **Data exposure:** Anonymous FTP and writable SMB shares enable unauthorized file access and modification — high risk for leakage and tampering.
- **Credential risk:** Weak/guessable credentials (e.g., `user:user`) make initial compromise trivial and allow escalation.
- **Remote compromise:** Outdated services and exposed admin panels are high-risk entry points for remote code execution and privilege escalation.
- **Attack chain example:** Anonymous FTP → upload web shell (if upload allowed) → execute via HTTP → pivot to SMB or extract credentials.

---

## ✅ Recommendations (Remediation / Hardening)
1. **Disable anonymous FTP** or restrict it strictly to a read-only, well-audited public folder with no upload capability.
2. **Remove default/weak accounts** and enforce strong passwords and account lockout policies. Use multifactor authentication where possible.
3. **Harden SMB shares:** Remove anonymous write access, restrict shares to specific authenticated users/groups, and enable auditing.
4. **Patch and update web stacks:** Upgrade Apache, Tomcat, PHP, and web applications to supported versions; remove sample/demo apps from production.
5. **Segment networks:** Keep intentionally vulnerable lab VMs isolated from production and restrict admin interfaces to private management networks.
6. **Enable monitoring & alerting:** Centralize logs, use FIM (file integrity monitoring), and alert on suspicious uploads or unusual authentication events.
7. **Perform regular vulnerability scans and timely remediation** to reduce attack surface.

---

## Files & Evidence
- **Scans:** `scans/scan1_sv_sc.txt`, `scans/scan2_ftp.txt`, `scans/scan3_smb.txt`, `scans/scan4_http.txt`  
- **Screenshots:** `images/breached_ftp.png`, `images/ip_adr.png`, `images/metasploitable_webpage.png`

---

## Notes & Ethics
- Only perform these scans and actions on lab systems you own or have explicit authorization to test. Unauthorized scanning or exploitation is illegal and unethical.
- Metasploitable2 and similar VMs are intentionally vulnerable and intended for learning—do not expose such VMs to untrusted networks.

---

## Author
**Zach Maestas**  
CSU – Computer Science (Networks & Security Concentration)

