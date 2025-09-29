# Homework 4 – Host Discovery & Service Enumeration (Nmap)
**Course:** CS 456 – Modern Cybersecurity  
**Repo path:** `modern-cybersecurity/homeworks/hw4`

---

## 📌 Lab Objective
Perform host discovery and service enumeration against the target container (`10.89.0.5`) using `nmap`. Goals:  
- Discover open TCP and UDP ports.  
- Identify running services and versions.  
- Attempt OS detection.  
- Interpret results and answer analysis questions.  

All screenshots are stored in this folder’s `images/` subdirectory.

---

## 🖼️ Screenshots
- `images/ip_adr.png` — Interface / `ip a` output  
- `images/tcp_scan.png` — Full TCP port scan (`nmap -p-`)  
- `images/service_version_detection_scan.png` — Service/version detection (`nmap -sV`)  
- `images/os_detection_scan.png` — OS detection (`sudo nmap -O`)  
- `images/udp_scan.png` — UDP top-ports scan (`sudo nmap -sU --top-ports 20`)

---

## 💻 Commands Run
```bash
# Full TCP port discovery (scan all TCP ports)
nmap -p- 10.89.0.5 -oN scans/tcp_full.txt

# Service/version detection for discovered ports
nmap -sV -p- 10.89.0.5 -oN scans/service_versions.txt

# OS detection (requires sudo/root)
sudo nmap -O 10.89.0.5 -oN scans/os_detection.txt

# UDP top-ports scan (requires sudo/root)
sudo nmap -sU --top-ports 20 10.89.0.5 -oN scans/udp_top20.txt
