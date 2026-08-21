# -networkwalks-B082-Week2-Footprinting-Network-Scanning-
Practical footprinting, reconnaissance, and network scanning report using Kali Linux tools (WHOIS, WhatWeb, Nslookup, Curl, Wafw00f, DNSRecon) and Zenmap for host discovery and network topology mapping.
<p align="center">
  <img src="https://img.shields.io/badge/Penetration%20Testing-Report-blue?style=for-the-badge&logo=shield" alt="Pentest Report">
  <img src="https://img.shields.io/badge/Modules-W2--PM1%20%7C%20W2--PM5-orange?style=for-the-badge" alt="Modules">
  <img src="https://img.shields.io/badge/Platform-Kali%20Linux%20%2F%20VirtualBox-black?style=for-the-badge&logo=kali-linux" alt="Platform">
</p>

---

## 📋 Document Overview

| Metadata Field | Details |
| :--- | :--- |
| **Report Title** | Penetration Testing Report: Footprinting & Network Scanning Phases |
| **Document Code** | `W2-PM1` |
| **Pentester Name** | **Ng Jian Nian** |
| **Program / Batch** | B082 - Networkwalks Cybersecurity Internship |
| **Testing Period** | 17 August 2026 – 24 August 2026 |
| **Modules Covered** | `W2-PM1` (Multiple Kali Tools), `W2-PM5` (Zenmap Scanning) & `W2-PM-FINAL` |
| **Client / Target** | 1. Networkwalks (`networkwalks.com`) <br> 2. Local Lab Environment (`10.0.0.0/24`) |
| **Authorization** | Secured Written Permission Verified (Yes) |
| **Phases Covered** | Phase 1: Reconnaissance & Footprinting <br> Phase 2: Scanning & Network Discovery |

---

## 📚 Internship Curriculum & Module Selection

Choose any 1 module from electives + both essential projects (total 3 modules minimum).

### Electives (choose at least 1):
* **W2-PM1:** Footprinting with multiple Kali tools (6 tools on `networkwalks.com`) **[SELECTED & COMPLETED]**
* **W2-PM2:** GHDB based Footprinting Attacks
* **W2-PM3:** Maltego based Footprinting Attacks
* **W2-PM4:** theHarvester based Footprinting Attacks

### Essentials (both required):
* **W2-PM5:** Zenmap based Network Scanning **[COMPLETED]**
* **W2-PM-FINAL:** Write a detailed report covering the modules you chose **[COMPLETED]**

---

## ⚠️ Liability Disclaimer

> **IMPORTANT LEGAL NOTICE**  
> This penetration testing report and all associated activities conducted between 17th August 2026 and 24th August 2026 were performed strictly within the authorized scope on systems for which explicit written permission was secured. The materials, findings, and methodologies detailed herein are provided by Networkwalks solely for educational, research, and authorized remediation purposes. Any unauthorized use, reproduction, or execution of these techniques outside the permitted scope is strictly prohibited and violates applicable laws. Neither the authors, the instructors, nor Networkwalks assume any liability or responsibility for misuse or unlawful actions taken using this knowledge; all actions and consequences remain the sole responsibility of the user.

---

## 🔍 Executive Summary & Introduction

This report presents the findings of a penetration testing engagement conducted between **17 August 2026 and 24 August 2026**. The assessment focused on passive/semi-passive reconnaissance against `networkwalks.com` alongside active network discovery within a dedicated personal pentesting laboratory. 

Covering core modules **W2-PM1** (*Multiple Kali Tools*) and **W2-PM5** (*Zenmap Scanning*), this document outlines the tools, execution steps, technical findings, and risk mitigations across both target environments.

---

## 🛠️ Tools Deployed & Technical Purpose

| Tool | Environment | Purpose |
| :--- | :--- | :--- |
| **Kali Linux** | Virtual Box | Primary OS platform hosting security testing applications |
| **Windows 10** | VirtualBox / Host | Local endpoint OS for network command execution & Zenmap hosting |
| **WHOIS** | Kali Terminal | Queries public registrar records for domain metadata & name servers |
| **WhatWeb** | Kali Terminal | Fingerprints web technology stack (CMS, server type, plugins, headers) |
| **Nslookup** | Kali Terminal | Direct DNS queries to resolve hostnames to IP addresses |
| **Curl (`curl -I`)** | Kali Terminal | Inspects HTTP/HTTPS response headers for server disclosure & API endpoints |
| **Wafw00f** | Kali Terminal | Detects and fingerprints Web Application Firewall (WAF) products |
| **DNSRecon** | Kali Terminal | Performs structured enumeration of DNS records (NS, MX, SPF, TXT, SRV) |
| **Zenmap (Nmap GUI)**| Windows / Kali | Sweeps local network subnets to discover live hosts, IPs, and MAC addresses |
| **Windows CMD** | Windows | Utility for local IP (`ipconfig`) and adapter validation |

---

## ⚡ Activities Performed

### Phase 1: Footprinting & Reconnaissance

I performed footprinting and reconnaissance against the `networkwalks.com` domain and local lab environment using Kali Linux hosted inside VirtualBox, utilizing six primary reconnaissance tools alongside Zenmap for discovery: **WHOIS**, **WhatWeb**, **Nslookup**, **Curl**, **Wafw00f**, **DNSRecon**, and **Zenmap (Nmap GUI)**. Each tool was used to collect a different type of information about the target assets:

1. **WHOIS Query:** Retrieved publicly available domain registration information and identified the primary name servers (`NS6135.HOSTGATOR.COM`, `NS6136.HOSTGATOR.COM`).
2. **WhatWeb Analysis:** Fingerprinted the website stack, exposing WordPress `7.0.4`, WP Download Manager `3.3.58`, Bootstrap `7.0.4`, and Apache web server metadata.
3. **Nslookup Resolution:** Directly queried DNS servers to resolve `networkwalks.com` to its public IP address (`192.232.216.135`).
4. **Curl Header Inspection:** Executed `curl -I https://networkwalks.com` to fetch HTTP response headers, revealing server banners and the active REST API endpoint (`/wp-json/`).
5. **Wafw00f WAF Detection:** Analyzed response behavior to identify active perimeter protection, detecting **ModSecurity (SpiderLabs)** as the primary Web Application Firewall.
6. **DNSRecon Enumeration:** Extracted full DNS zone records including Name Servers (NS), Mail Exchange (`mail.networkwalks.com`), SPF records, and active SRV records for cPanel autodiscovery services.

---

### Phase 2: Network Scanning with Zenmap

For the second activity, I used **Zenmap** to perform network discovery on my local network within VirtualBox. The practical required identifying the local IP configuration, sweeping the subnet for live hosts, capturing physical MAC addresses, and generating a network topology diagram:

1. Executed `ipconfig` on the Windows host to determine the active IPv4 address (`10.0.0.10`) and subnet mask (`255.255.255.0` / `/24`).
2. Swept the subnet target `10.0.0.10/24` using Zenmap's **Ping Scan** (`nmap -sn 10.0.0.10/24`) to identify active nodes.
3. Discovered **5 live hosts** across the local virtual network segment:

| IP Address | Physical MAC Address | Interface Vendor / Host Details |
| :---: | :---: | :--- |
| `10.0.0.1` | `52:54:00:12:35:00` | QEMU Virtual NIC (Gateway Node) |
| `10.0.0.2` | `08:00:27:18:BE:7F` | Oracle VirtualBox Virtual NIC |
| `10.0.0.10` | *Local Host Interface* | Windows Endpoint (Local Host) |
| `10.0.0.11` | `08:00:27:D2:2F:BD` | Oracle VirtualBox Virtual NIC |
| `10.0.0.16` | `08:00:27:8A:F2:13` | Oracle VirtualBox Virtual NIC |

4. Rendered and exported the visual interactive **Topology Diagram** from Zenmap in PDF graphic format with the legend enabled.

---

## 📊 Risk Analysis & Findings Matrix

| # | Risk / Finding | Evidence / Observation | Potential Impact | Risk Level |
| :-: | :--- | :--- | :--- | :-: |
| **1** | **Web technology information exposed** | WhatWeb identified WordPress `7.0.4` and WP Download Manager `3.3.58` | Threat actors can target version-specific vulnerabilities in exposed plugins or core CMS files | 🟡 Medium |
| **2** | **Server IP address identifiable** | Nslookup mapped `networkwalks.com` to `192.232.216.135` | Discloses server network location, enabling direct IP probing | 🟢 Low |
| **3** | **HTTP technical information exposed** | Curl returned Apache headers & exposed REST API (`/wp-json/`) | Assists automated fingerprinting and API endpoint enumeration | 🟢 Low |
| **4** | **WAF technology identifiable** | Wafw00f fingerprinted ModSecurity (SpiderLabs) | Reveals security controls, enabling attackers to craft specific bypass payloads | 🟢 Low |
| **5** | **DNS infrastructure information exposed** | DNSRecon enumerated MX, NS, SPF, and cPanel SRV records | Facilitates broader infrastructure profiling and social engineering targets | 🟡 Medium |
| **6** | **Multiple live hosts visible on local network** | Zenmap identified 5 active endpoints on `10.0.0.0/24` | Potential presence of rogue or unmonitored devices on internal subnets | 🟡 Medium |

> **Risk Rating Key:** 🔴 Critical | 🟡 Medium | 🟢 Low

---

## 🛡️ Recommendations & Actionable Remediation

- **Audit Exposed Technical Data:** Routinely review public-facing assets to restrict unnecessary visibility into CMS frameworks, plugins, and server software versions.
- **Maintain Patch Management:** Ensure WordPress, plugins (such as WP Download Manager), and underlying server stacks are kept updated against active CVE advisories.
- **Sanitize HTTP Headers:** Configure Apache/Nginx web servers to suppress version tokens (`ServerSignature Off`, `ServerTokens Prod`) and limit REST API information disclosure.
- **Audit DNS Records Periodically:** Clean up stale DNS entries and limit publicly published SRV/TXT records to essential services only.
- **Fine-Tune WAF Rules:** Continuously tune ModSecurity rule sets to block automated scanners and hide verbose signature headers.
- **Perform Regular Internal Network Audits:** Conduct scheduled host sweeps across internal subnets to ensure accurate asset management.
- **Investigate Unidentified Endpoints:** Isolate and verify any unauthorized or unrecognized MAC address connected to the network segment.
- **Maintain Network Topology Documentation:** Keep network topology maps and IP assignment inventories up to date.
- **Strict Scope Authorization:** Ensure all testing activities strictly adhere to pre-approved scope boundaries and legal agreements.

---

## 📸 Technical Evidence & Screen Captures

### 1. WHOIS Reconnaissance Output
![WHOIS Output 1](https://raw.githubusercontent.com/NgJianNian/Footprinting-Network-Scanning/main/evidences/whois_1.png)
![WHOIS Output 2](https://raw.githubusercontent.com/NgJianNian/Footprinting-Network-Scanning/main/evidences/whois_2.png)
![WHOIS Output 3](https://raw.githubusercontent.com/NgJianNian/Footprinting-Network-Scanning/main/evidences/whois_3.png)

---

### 2. WhatWeb Web Fingerprinting
![WhatWeb Output](https://raw.githubusercontent.com/NgJianNian/Footprinting-Network-Scanning/main/evidences/whatweb.png)

---

### 3. Nslookup IP Mapping
![Nslookup Output](https://raw.githubusercontent.com/NgJianNian/Footprinting-Network-Scanning/main/evidences/nslookup.png)

---

### 4. Curl Response Headers (`curl -I`)
![Curl Output](https://raw.githubusercontent.com/NgJianNian/Footprinting-Network-Scanning/main/evidences/curl.png)

---

### 5. Wafw00f WAF Identification
![Wafw00f Output](https://raw.githubusercontent.com/NgJianNian/Footprinting-Network-Scanning/main/evidences/wafw00f.png)

---

### 6. DNSRecon Zone Enumeration
![DNSRecon Output](https://raw.githubusercontent.com/NgJianNian/Footprinting-Network-Scanning/main/evidences/dnsrecon.png)

---

### 7. Windows IP Configuration (`ipconfig`)
![ipconfig Output](https://raw.githubusercontent.com/NgJianNian/Footprinting-Network-Scanning/main/evidences/ipconfig.png)

---

### 8. Zenmap Subnet Ping Scan
![Zenmap Scan Output](https://raw.githubusercontent.com/NgJianNian/Footprinting-Network-Scanning/main/evidences/nmap_scan.png)

---

### 9. Zenmap Network Topology Render
![Zenmap Topology Output](https://raw.githubusercontent.com/NgJianNian/Footprinting-Network-Scanning/main/evidences/topology.png)

---

## 🎓 Internship Conclusion & Reflection

During the second week of my Cybersecurity & Ethical Hacking internship, I carried out hands-on exercises focused on footprinting, intelligence gathering, and active network scanning.

The footprinting phase involved utilizing six core Kali Linux utilities to evaluate the target domain. This demonstrated how WHOIS reveals domain registration metadata, WhatWeb uncovers the underlying web technology stack, Nslookup determines IP mappings, Curl exposes HTTP response headers, Wafw00f detects perimeter Web Application Firewalls, and DNSRecon extracts comprehensive DNS zone details.

The subsequent network scanning phase relied on Zenmap to analyze local network settings, sweep for live endpoints, document IP and physical MAC addresses, and render a visual topology map of the environment.

These practical tasks highlighted that preliminary information gathering forms the foundation of any security evaluation. Well before launching exploit attempts, an analyst can gain deep visibility into a system's exposure merely by evaluating public records and passive network traffic. Furthermore, the experience emphasized the need for precise technical reporting—effective documentation must detail the methodology used, specific observations made, the security risks introduced, and practical steps to mitigate those threats.

Ultimately, these exercises reinforced that all scanning and intelligence-gathering efforts must strictly adhere to legal boundaries and written authorization. Every task described in this document was performed purely for educational purposes within a controlled laboratory setting.

---

## 👤 Author Information

* **Name:** **Ng Jian Nian**
* **Role:** Cybersecurity Professional | Batch `B082`
* **Program:** Networkwalks Cybersecurity Internship Program
* **LinkedIn:** [Jian Nian Ng on LinkedIn](https://www.linkedin.com/in/jian-nian-ng-5083a2387)

---
*Report published as part of Week 02 practical submission for Networkwalks Academy.*
