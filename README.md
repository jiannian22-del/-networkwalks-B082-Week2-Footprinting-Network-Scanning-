<p align="center">
  <img src="[https://img.shields.io/badge/Penetration%20Testing-Report-blue?style=for-the-badge&logo=shield](https://img.shields.io/badge/Penetration%20Testing-Report-blue?style=for-the-badge&logo=shield)" alt="Pentest Report">
  <img src="[https://img.shields.io/badge/Modules-W2--PM1%20%7C%20W2--PM5-orange?style=for-the-badge](https://img.shields.io/badge/Modules-W2--PM1%20%7C%20W2--PM5-orange?style=for-the-badge)" alt="Modules">
  <img src="[https://img.shields.io/badge/Platform-Kali%20Linux%20%2F%20VirtualBox-black?style=for-the-badge&logo=kali-linux](https://img.shields.io/badge/Platform-Kali%20Linux%20%2F%20VirtualBox-black?style=for-the-badge&logo=kali-linux)" alt="Platform">
</p>

---

## 📋 Document Overview

| Metadata Field | Details |
| :--- | :--- |
| **Report Title** | Penetration Testing Report: Footprinting & Network Scanning Phases |
| **Document Code** | `W2-PM-FINAL` |
| **Pentester Name** | **Ng Jian Nian** |
| **Program / Batch** | B082 - Networkwalks Cybersecurity Internship |
| **Testing Period** | 17 August 2026 – 24 August 2026 |
| **Modules Covered** | `W2-PM1` (Multiple Kali Tools) & `W2-PM5` (Zenmap Scanning) |
| **Client / Target** | 1. Networkwalks (`networkwalks.com`) <br> 2. Local Lab Environment (`10.0.0.0/24`) |
| **Authorization** | Secured Written Permission Verified (Yes) |
| **Phases Covered** | Phase 1: Reconnaissance & Footprinting <br> Phase 2: Scanning & Network Discovery |

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
| **Kali Linux** | VirtualBox | Primary OS platform hosting security testing applications |
| **Windows 10** | VirtualBox / Host | Local endpoint OS for network command execution & Zenmap hosting |
| **WHOIS** | Kali Terminal | Queries public registrar records for domain metadata & name servers |
| **WhatWeb** | Kali Terminal | Fingerprints web technology stack (CMS, server type, plugins, headers)|
| **Nslookup** | Kali Terminal | Direct DNS queries to resolve hostnames to IP addresses[cite: 3, 4] |
| **Curl (`curl -I`)** | Kali Terminal | Inspects HTTP/HTTPS response headers for server disclosure & API endpoints |
| **Wafw00f** | Kali Terminal | Detects and fingerprints Web Application Firewall (WAF) products |
| **DNSRecon** | Kali Terminal | Performs structured enumeration of DNS records (NS, MX, SPF, TXT, SRV) |
| **Zenmap (Nmap GUI)**| Windows / Kali | Sweeps local network subnets to discover live hosts, IPs, and MAC addresses |
| **Windows CMD** | Windows | Utility for local IP (`ipconfig`) and adapter validation |

---

## ⚡ Activities Performed & Module Breakdown

### 🎯 Module W2-PM1: Footprinting & Reconnaissance

This phase involved passive and semi-passive reconnaissance against `networkwalks.com` using six built-in Kali Linux tools.

#### Task 1: WHOIS Domain Registration Lookup
* **Code / Command Used:**
  `whois networkwalks.com`
* **Explanation:** Queries public registrar databases to retrieve domain owner registration details, creation/expiry dates, registrar contact details, and assigned name servers.
* **Attacker Perspective:** Discovers domain registrar, expiration dates, and hosting provider (e.g., HostGator via name servers). Useful for social engineering or tracking infrastructure changes.
* **Observed Findings:**
  * Target Domain: `networkwalks.com`
  * Primary Name Servers: `NS6135.HOSTGATOR.COM`, `NS6136.HOSTGATOR.COM`

---

#### Task 2: WhatWeb Web Technology Fingerprinting
* **Code / Command Used:**
  `whatweb networkwalks.com`
* **Explanation:** Analyzes target web responses to identify technologies running on the site, including web servers, CMS platforms, third-party plugins, frameworks, and IP addresses.
* **Attacker Perspective:** Reveals exact software types and version numbers (e.g., WordPress core, plugin versions). Attackers cross-reference these versions with vulnerability databases (e.g., CVEs, Exploit-DB) to locate targetable exploits.
* **Observed Findings:**
  * CMS & Versioning: WordPress `7.0.4`, WP Download Manager `3.3.58`, Bootstrap `7.0.4`
  * Server Stack: Apache web server

---

#### Task 3: Nslookup Host DNS Resolution
* **Code / Command Used:**
  `nslookup networkwalks.com`
* **Explanation:** Queries configured DNS servers directly to translate domain hostnames into public IPv4 addresses.
* **Attacker Perspective:** Translates a user-friendly domain name into the direct IPv4 address. Allows direct IP scanning, discovery of co-hosted sites on shared servers, and infrastructure mapping.
* **Observed Findings:**
  * Target Resolved IP: `192.232.216.135`

---

#### Task 4: Curl HTTP Response Banners (`curl -I`)
* **Code / Command Used:**
  `curl -I [https://networkwalks.com](https://networkwalks.com)`
* **Explanation:** Sends an HTTP HEAD request to fetch response headers without downloading full web page body content.
* **Attacker Perspective:** Uncovers underlying web server banners, active caching layers, session cookies, and hidden structural endpoints (such as `/wp-json/` REST API endpoints).
* **Observed Findings:**
  * Target Host / Domain: `networkwalks.com`
  * API Endpoint Path: `/wp-json/`
  * Server Banners: Apache Server response headers

---

#### Task 5: Wafw00f Perimeter Firewall Detection
* **Code / Command Used:**
  `wafw00f networkwalks.com`
* **Explanation:** Sends custom HTTP requests and analyzes server response behavior to detect perimeter Web Application Firewalls (WAF).
* **Attacker Perspective:** Informs the attacker if active security filters are inspecting traffic. Helps attackers choose whether to use WAF-bypass payloads or adjust scanning speed to avoid detection.
* **Observed Findings:**
  * Active Firewall Product: ModSecurity (SpiderLabs)

---

#### Task 6: DNSRecon Zone Record Enumeration
* **Code / Command Used:**
  `dnsrecon -d networkwalks.com`
* **Explanation:** Enumerates DNS records, collecting Name Server (NS), Mail Exchange (MX), Sender Policy Framework (SPF/TXT), and Service (SRV) records.
* **Attacker Perspective:** Maps out email servers, mail handling rules, DNS software versions, and cPanel administrative autodiscovery endpoints.
* **Observed Findings:**
  * Mail Server Host: `mail.networkwalks.com``192.232.216.131`
  * Record Types Discovered: NS, MX, SPF, and active SRV records for cPanel autodiscovery services

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

### 🎯 Module W2-PM5: Active Network Scanning with Zenmap

This phase involved active network discovery across local virtual networks using **Zenmap**.

#### Task 1: Download & Install Zenmap
* Installed Zenmap on the host platform for local subnet scanning.
  <a>https://nmap.org/download</a>

#### Task 2: Validate Local Adapter IP & Subnet
* Executed `ipconfig` on the Windows host terminal to verify active local IPv4 networking parameters.
* **Observed Findings:**
  * Host Endpoint IPv4 Address: `10.0.0.10`
  * Subnet Mask: `255.255.255.0` (`10.0.0.0/24`)

#### Task 3–6: Subnet Host Discovery Sweep
* **Command Executed:**
  `nmap -sn 10.0.0.10/24`
* Executed a **Ping Scan** across the local network segment (`10.0.0.10/24`) to identify active nodes, IP addresses, and physical MAC addresses.

| IP Address | Physical MAC Address | Interface Vendor / Host Details |
| :---: | :---: | :--- |
| `10.0.0.1` | `52:54:00:12:35:00` | QEMU Virtual NIC (Gateway Node) |
| `10.0.0.2` | `08:00:27:18:BE:7F` | Oracle VirtualBox Virtual NIC |
| `10.0.0.10` | *Local Host Interface* | Windows Endpoint (Local Host) |
| `10.0.0.11` | `08:00:27:D2:2F:BD` | Oracle VirtualBox Virtual NIC |
| `10.0.0.16` | `08:00:27:8A:F2:13` | Oracle VirtualBox Virtual NIC |
<img width="1041" height="963" alt="nmap scan" src="https://github.com/user-attachments/assets/757df225-c240-4e60-aafa-01faa068301f" />

#### Task 7: Render & Export Network Topology Graphic
* Rendered an interactive visual **Topology Map** in Zenmap with the legend enabled, saving the graphic output in PDF format.
<img width="1050" height="954" alt="topology 2" src="https://github.com/user-attachments/assets/48d86df8-46c3-4f84-b0e4-b7031d14bf69" />

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
![WHOIS Output 1]<img width="1340" height="921" alt="Whois 1" src="https://github.com/user-attachments/assets/8fa35adc-7f7e-4aa6-927f-0342ed23ddcd" />
![WHOIS Output 2]<img width="1351" height="927" alt="Whois 2" src="https://github.com/user-attachments/assets/b006ed90-6886-41f2-81f2-3730ae49bc90" />
![WHOIS Output 3]<img width="1305" height="908" alt="Whois 3" src="https://github.com/user-attachments/assets/98f9661d-b1d9-43b8-8ed1-4066c7933933" />

---

### 2. WhatWeb Web Fingerprinting
![WhatWeb Output]<img width="1303" height="923" alt="Whatweb 1" src="https://github.com/user-attachments/assets/ebb7c085-28ab-47e9-8132-5bb1b2128144" />

---

### 3. Nslookup IP Mapping
![Nslookup Output]<img width="1311" height="921" alt="nslookup 1" src="https://github.com/user-attachments/assets/9a362188-a987-4860-ab27-94a69fc5c9f4" />

---

### 4. Curl Response Headers (`curl -I`)
![Curl Output]<img width="1298" height="928" alt="Curls 1" src="https://github.com/user-attachments/assets/67888433-dc33-4a1f-ab57-d8e1c5f20682" />

---

### 5. Wafw00f WAF Identification
![Wafw00f Output]<img width="1323" height="927" alt="warw00f 1" src="https://github.com/user-attachments/assets/94287c33-58d2-403f-a923-537beff6b2c6" />

---

### 6. DNSRecon Zone Enumeration
![DNSRecon Output]<img width="1304" height="919" alt="dnsrecon 1" src="https://github.com/user-attachments/assets/3cc30147-144e-479a-9479-1f7ebf22d93f" />

---

### 7. Windows IP Configuration (`ipconfig`)
![ipconfig Output]<img width="1040" height="957" alt="ipconfig" src="https://github.com/user-attachments/assets/c5a30a47-ccee-4751-8a5a-46856af5395c" />

---

### 8. Zenmap Subnet Ping Scan
![Zenmap Scan Output]<img width="1041" height="963" alt="nmap scan" src="https://github.com/user-attachments/assets/757df225-c240-4e60-aafa-01faa068301f" />

---

### 9. Zenmap Network Topology Render
![Zenmap Topology Output]<img width="1050" height="954" alt="topology 2" src="https://github.com/user-attachments/assets/48d86df8-46c3-4f84-b0e4-b7031d14bf69" />


---

## 🎓 Internship Conclusion & Reflection

During the second week of my Cybersecurity & Ethical Hacking internship, I carried out hands-on exercises focused on footprinting, intelligence gathering, and active network scanning.

The footprinting phase involved utilizing six core Kali Linux utilities to evaluate the target domain. This demonstrated how WHOIS reveals domain registration metadata, WhatWeb uncovers the underlying web technology stack, Nslookup determines IP mappings, Curl exposes HTTP response headers, Wafw00f detects perimeter Web Application Firewalls, and DNSRecon extracts comprehensive DNS zone details.

The subsequent network scanning phase relied on Zenmap to analyze local network settings, sweep for live endpoints, document IP and physical MAC addresses, and render a visual topology map of the environment.

These practical tasks highlighted that preliminary information gathering forms the foundation of any security evaluation. Well before launching exploit attempts, an analyst can gain deep visibility into a system's exposure merely by evaluating public records and passive network traffic. Furthermore, the experience emphasized the need for precise technical reporting—effective documentation must detail the methodology used, specific observations made, the security risks introduced, and practical steps to mitigate those threats.

Finally, these exercises reinforced that all scanning and intelligence-gathering efforts must strictly adhere to legal boundaries and written authorization. Every task described in this document was performed purely for educational purposes within a controlled laboratory setting.

---

## 👤 Author Information

* **Name:** **Ng Jian Nian**
* **Role:** Cybersecurity Professional | Batch `B082`
* **Program:** Networkwalks Cybersecurity Internship Program
* **LinkedIn:** [Jian Nian Ng on LinkedIn](https://www.linkedin.com/in/jian-nian-ng-5083a2387)

---
