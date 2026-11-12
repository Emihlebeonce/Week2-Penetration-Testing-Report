# Week2-Penetration-Testing-Report
Week 2 Penetration Testing Report - GHDB Footprinting (W2-PM2) + Zenmap Network Scanning (W2-PM5) by Emihle Memela

# PENETRATION TESTING REPORT
## Week 2 Project Work

**Name:** Emihle Memela
**Date:** 12 November 2026
**Modules Completed:**
- W2-PM2: GHDB based Footprinting Attacks
- W2-PM5: Zenmap based Network Scanning
- W2-PM-FINAL: Final Report

---

## Table of Contents
1. Introduction
2. W2-PM2: GHDB based Footprinting Attacks
3. W2-PM5: Zenmap based Network Scanning
4. Conclusion

---

## 1. Introduction

This report covers the Week 2 penetration testing project work. Two modules were completed: GHDB based footprinting and Zenmap based network scanning.

The objective was to understand how attackers use publicly available information and network scanning tools to gather information about a target before launching an attack. All testing was done for educational purposes on authorized targets only.

---

## 2. W2-PM2: GHDB BASED FOOTPRINTING ATTACKS

Google Hacking Database (GHDB) is a database on Exploit-DB that contains Google Dorks. A Google Dork is a special search query used to find sensitive information that is publicly exposed on the internet.

Three dorks were tested on Exploit-DB GHDB and verified on Bing search engine.

### 2.1 Dork 1: inurl:home.htm intitle:1766

This dork is used to find device control panel / admin pages.

![Screenshot 1 - GHDB inurl:home.htm](screenshots/01-GHDB-home-htm.png)

The same dork was then tested on Bing to confirm that the information is not only available on Google.

![Screenshot 2 - Bing inurl:home.htm](screenshots/02-Bing-home-htm.png)

**Finding:** The search results show device home pages / control panels that are exposed publicly. An attacker can use this to find devices with default login pages.

**Risk:** High - If the device uses default passwords, an attacker can gain access.
**Remediation:** Change default passwords, restrict admin pages to internal network only, and remove device pages from public search indexing.

### 2.2 Dork 2: intitle:index of /etc/ssh

This dork is used to find open directory listings exposing /etc/ssh files.

![Screenshot 3 - GHDB etc ssh](screenshots/03-GHDB-etc-ssh.png)

![Screenshot 4 - Bing etc ssh](screenshots/04-Bing-etc-ssh.png)

**Finding:** The results show open directories that list SSH configuration files such as ssh_host keys and ssh_config.

**Risk:** High - SSH keys and configuration files can help an attacker understand the server setup and potentially gain unauthorized access.
**Remediation:** Disable directory listing on web servers, remove sensitive files from public folders, and set correct file permissions.

### 2.3 Dork 3: "Header for logs at time" ext:log

This dork is used to find publicly exposed log files.

![Screenshot 5 - GHDB logs](screenshots/05-GHDB-logs.png)

![Screenshot 6 - Bing logs](screenshots/06-Bing-logs.png)

**Finding:** The results show log files that contain headers, timestamps, error messages and system information.

**Risk:** Medium - Log files can reveal internal IP addresses, usernames, software versions and system errors that help an attacker plan an attack.
**Remediation:** Do not store log files in public web folders. Use authentication and restrict access to logs.

**Conclusion for W2-PM2:** GHDB shows that a lot of sensitive information is exposed publicly without hacking. Verification on Bing proves that the risk is not limited to one search engine. Organizations must protect sensitive files and directories from public search engines.

---

## 3. W2-PM5: ZENMAP BASED NETWORK SCANNING

Zenmap is the graphical user interface for Nmap. It was used to scan the authorized target scanme.nmap.org.

**Target:** scanme.nmap.org (45.33.32.156)
**Command:** nmap -T4 -A -v scanme.nmap.org
**Profile:** Intense Scan

### 3.1 Nmap Output

![Screenshot 7 - Zenmap Output](screenshots/07-Zenmap-output.png)

**Finding:**
The scan found the following open ports:
- 22/tcp open - ssh - OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13
- 80/tcp open - http - Apache httpd 2.4.7 (Ubuntu)
- 9929/tcp open - nping-echo - Nping echo

Most other ports were shown as filtered, which means they are blocked by a firewall.

### 3.2 Ports / Hosts

![Screenshot 8 - Zenmap Ports Hosts](screenshots/08-Zenmap-ports.png)

**Finding:** The Ports / Hosts tab shows the list of hosts scanned and the status of each port. Only 3 ports are open, the rest are filtered.

### 3.3 Topology

![Screenshot 9 - Zenmap Topology](screenshots/09-Zenmap-topology.png)

**Finding:** The topology shows a direct connection: localhost -> scanme.nmap.org (45.33.32.156). There are no intermediate hops shown.

**Risk:** SSH (port 22) and HTTP (port 80) are common attack targets. If they use old software versions or weak passwords, an attacker can exploit them.
**Remediation:** Update OpenSSH and Apache to the latest versions, use strong passwords and key-based authentication for SSH, disable unnecessary services, and keep the firewall enabled.

**Conclusion for W2-PM5:** The Zenmap scan shows that the target has a small attack surface with only 3 open ports, and the firewall is working. However, the open services must still be hardened and patched regularly.

---

## 4. Conclusion

This project showed two important phases of penetration testing:
1. Footprinting with GHDB - attackers can find sensitive information without directly touching the target, using only search engines.
2. Network scanning with Zenmap - attackers can discover open ports, services and operating systems on a target.

Both modules prove that information exposure is a serious risk. The best defence is to remove sensitive files from public access, disable directory listing, use strong authentication, patch services, and keep firewalls enabled.

