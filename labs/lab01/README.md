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
