# metasploitable-network-security-assessment
Controlled network security assessment of a deliberately vulnerable Metasploitable system using Kali Linux and Nmap.

## Project Overview

This project documents a controlled network security assessment of a deliberately vulnerable Metasploitable system. The assessment was performed in an isolated virtual lab using Kali Linux and Nmap.

The goal of this project was to identify exposed network services, determine the software running on those services, understand potential security risks, and recommend basic defensive measures.

## Lab Environment

- **Attacker/Analysis System:** Kali Linux
- **Target System:** Metasploitable
- **Target IP Address:** 10.0.2.8
- **Primary Tool:** Nmap
- **Lab Type:** Isolated virtual lab environment

The assessment was performed against a deliberately vulnerable system in a controlled lab environment for cybersecurity training purposes.

## 1. Connectivity Test

Before performing the network scan, I tested connectivity to the Metasploitable system using ICMP.

### Command

```bash 
ping -c 4 10.0.2.8
```

### Result

The target responded successfully to all four ICMP requests.

- **Packets Sent:** 4
- **Packets Received:** 4
- **Packet Loss:** 0%
- **Average Response Time:** 0.927 ms

This confirmed that the Metasploitable system was reachable from the Kali Linux system before beginning the Nmap assessment.

## 2. Nmap Port Scan

After confirming connectivity, I performed a basic Nmap scan to identify open TCP ports on the Metasploitable system.

### Command

```bash
nmap 10.0.2.8
```

### Result

The scan showed that the target was up and had **23 open TCP ports** out of the default 1,000 TCP ports scanned. The remaining **977 ports were closed**.

The open ports showed that the system was running several network services that could increase its attack surface.

### Important Open Ports and Services

| Port | Service | Security Concern |
|---:|---|---|
| **21/tcp** | FTP | FTP can transmit data without encryption and should be secured or replaced with SFTP when appropriate. |
| **22/tcp** | SSH | Provides remote access and should be properly secured and restricted. |
| **23/tcp** | Telnet | Sends information without encryption and should generally be replaced with SSH. |
| **80/tcp** | HTTP | Web traffic is not encrypted and should use HTTPS when sensitive information is involved. |
| **139/tcp** | NetBIOS | Can expose network file-sharing services and should be restricted when not needed.|
| **445/tcp** | SMB | File and printer sharing can be targeted if improperly secured or exposed. |
| **1524/tcp** | Bindshell / Root Shell | Provides a root-level remote shell and represents a serious security risk if exposed. |
| **2049/tcp** | NFS | Network file sharing can expose files if access controls are weak. |
| **3306/tcp** | MySQL | Database access should be restricted to authorized systems and users. |

## 3. Service and Version Detection

After identifying the open ports, I used Nmap service and version detection to identify the software running on the exposed services.

### Command

```bash
nmap -sV 10.0.2.8
```

### Results

The service and version scan identified several services and the software versions running on the Metasploitable system.

| Port | Service | Detected Software/Version |
|---:|---|---|
| **21/tcp** | FTP | vsftpd 2.3.4 |
| **22/tcp** | SSH | OpenSSH 4.7p1 |
| **23/tcp** | Telnet | Linux telnetd |
| **25/tcp** | SMTP | Postfix smtpd |
| **53/tcp** | DNS | ISC BIND 9.4.2 |
| **80/tcp** | HTTP | Apache 2.2.8 |
| **111/tcp** | RPC | rpcbind 2 |
| **1524/tcp** | Bindshell | Metasploitable root shell |


Several of the detected services are running older software versions that may contain known security weaknesses.

## 4. Key Security Finding: Port 1524/tcp Root Bindshell

One of the most significant findings from the Nmap scan was the open **1524/tcp** port. Nmap identified the service as a **bindshell** and described it as a **Metasploitable root shell**.

### Security Risk

A root shell provides highly privileged access to the system. If this service were exposed on a real system, an attacker could potentially use it to gain unauthorized access, modify system settings, access files, or disrupt system operations.

### Recommended Defense

- Disable the bindshell service if it is not required.
- Restrict access to unnecessary ports.
- Monitor systems for unexpected listening services.
- Investigate unknown services that provide remote access.
- Limit administrative privileges using the principle of least privilege.

## 5. Security Recommendations

Based on the Nmap scan results, several steps could be taken to improve the security of the Metasploitable system.

- **Disable unnecessary services:** Services that are not needed should be disabled to reduce the number of potential entry points.
- **Restrict network access:** Use firewall rules to limit access to services and ports to only authorized systems.
- **Replace insecure protocols:** Telnet and unencrypted FTP should be replaced with more secure alternatives such as SSH and SFTP when appropriate.
- **Update software:** Older services and software should be updated to supported versions to address known security weaknesses.
- **Protect database services:** MySQL should only be accessible from systems that require database access.
- **Investigate the root bindshell:** The bindshell on port 1524 should be disabled and investigated because it provides a root-level shell.
- **Monitor network services:** Systems should be monitored for unexpected open ports, services, or remote connections.

## 6. What I Learned 

This project gave me a better understanding of how network scanning can be used as a tool to identify potential security risks on a system. I learned how to use Nmap to check whether a system is reachable, identify open ports, and determine what services and software versions are running. One of my biggest takeaways was understanding that every unnecessary open service can create another potential entry point for an attacker. Finding that the root bindshell on port 1524 was vulnerable and could allow an attacker to gain a high level of control over the system and make significant changes made the risk much clearer to me. Completing this lab gave me more experience looking at scan results from a defensive perspective and thinking about how services can be secured or removed to reduce risk.

## 7. Disclaimer

This project was completed in a controlled, isolated lab environment using a deliberately vulnerable Metasploitable system for cybersecurity training purposes. No unauthorized systems were scanned or tested.

## 8. Screenshot/Evidence

The following screenshots show the Nmap commands and results from the controlled Metasploitable lab assessment.

### Connectivity Test 

The screenshot below shows the successful ping test confirming that the Metasploitable system was reachable from Kali Linux.

