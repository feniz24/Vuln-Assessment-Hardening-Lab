# Purple Team Lab: Vulnerability Assessment & System Hardening

## 🎯 Objective
This project demonstrates a full-cycle vulnerability management process. Rather than relying solely on automated scanners, I manually verified critical exploits in a sandboxed environment and applied application-level and network-level hardening to neutralize threats on unpatchable legacy systems.

## 🏗️ Lab Architecture
The environment was built in VirtualBox using an isolated NAT Network to safely execute exploits.
- **Attacker Machine:** Kali Linux
- **Target Machine:** Metasploitable 2 (Ubuntu Linux)
- **Tool Stack:** Nmap, Nikto, OpenVAS (Greenbone), Metasploit Framework, UFW.

## 🔍 Vulnerability Discovery & Verification
Initial reconnaissance was conducted using Nmap and Nikto, followed by a deep-dive vulnerability scan using OpenVAS. The initial scan revealed **10 Critical and 4 High vulnerabilities**. 

To eliminate false positives, I manually exploited the following critical findings:
1. **vsftpd 2.3.4 Backdoor (Port 21):** Exploited via Metasploit to achieve a root shell. *Note: Discovered that stopping the FTP service does not kill the lingering backdoor process on Port 6200.*
2. **OpenSSH Vulnerability (Port 22):** Successfully executed a custom exploit script (available in the `/scripts` directory).

## 🛡️ Remediation & System Hardening
Because the target system utilizes legacy software, standard package updates were not viable. I implemented the following hardening techniques:
- **Service Deprecation:** Commented out legacy `r-services` in `/etc/inetd.conf` and revoked execution permissions for `vsftpd` (`chmod -x`).
- **Access Control:** Enforced `PermitRootLogin no` in the SSH daemon configuration.
- **Application Hardening:** Taken vulnerable web applications offline by stripping file permissions (`chmod 000` on TWiki) and disabled the unused AJP protocol in Tomcat (`server.xml`) to mitigate **Ghostcat (CVE-2020-1938)**.
- **Network Defense:** Deployed UFW with a strict Default Deny policy, permitting only Port 22.

## 📊 Deliverables
The successful mitigation of these vulnerabilities is documented in the before-and-after OpenVAS reports:
- [📄 Initial Vulnerability Scan (Before Hardening)](reports/openvas_1st_scan.pdf)
- [📄 Final Vulnerability Scan (After Hardening)](reports/openvas_3rd_scan.pdf)

## 📝 Detailed Write-Up
For a complete walkthrough of the exploit verification and hardening process, read my full technical blog post on [Medium](https://medium.com/@feniztho/from-red-to-blue-a-practical-vulnerability-assessment-and-system-hardening-lab-8344a61de4c1)