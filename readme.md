# Privilege Escalation Project
## verview

In this project, the objective was to obtain root access on a provided virtual machine (VM) without visible IP addresses. The VM image 01-Local1.ova was used, and no modifications to GRUB or the VM configuration were allowed. The approach involved discovering the VM's IP address, scanning for open ports, identifying running services, and exploiting vulnerabilities to gain root access.
Prerequisites

-  VirtualBox: Ensure VirtualBox is installed and configured to run the provided VM.
-   Nmap: Network scanning tool.
-   Metasploit: For exploitation (optional, based on findings).
-   Additional Tools: tcpdump, Wireshark, nikto, OpenVAS, etc. (as needed).

## Setup

- Install the VM:
1. Download the VM image: 01-Local1.utm.zip (for Apple Silicon CPUs).
2. Import the VM into VirtualBox:
3. Open VirtualBox.
4. Go to File > Import Appliance.
5. Select the downloaded file and follow the prompts.

- Configure Networking:
Ensure the VM is set up with the appropriate network configuration (NAT or Bridged Adapter) to allow for network scanning.

- Steps
1. Discover the VM's IP Address

Network Scan: Use nmap to identify active IP addresses on the network:

```bash
nmap -sP 192.168.1.0/24
```

Replace 0.0.0.0/24 with the network range of your local network.

Advanced Scan: Once you identify possible IP addresses, perform a more detailed scan to confirm the VM's IP address:

```bash
nmap -sP -v <POTENTIAL_IP_ADDRESS>
```

2. Scan for Open Ports and Services

Port Scan: Scan all ports on the identified IP address:

```bash
nmap -sS -p- <IP_OF_THE_VM>
```

Service and Version Detection: Determine the services running and their versions:

```bash
nmap -sV <IP_OF_THE_VM>
```

3. Identify Vulnerabilities

Service Analysis: Based on the services and versions detected, research known vulnerabilities. Use databases like CVE and Exploit-DB to find relevant exploits.

Vulnerability Scanning: Optionally, use vulnerability scanning tools like nikto, OpenVAS, or Nessus for a comprehensive scan.

4. Exploit Vulnerabilities

Exploit Identification: If you find a vulnerable service, you may use tools like Metasploit to exploit it. Here’s an example command to use Metasploit for an exploit:

```bash
msfconsole
use exploit/multi/handler
set PAYLOAD <PAYLOAD_TYPE>
set LHOST <YOUR_IP>
set LPORT <YOUR_PORT>
exploit
```

Manual Exploitation: If no automated exploit is available, we might need to manually exploit the vulnerability based on the information you gathered.

5. Escalate Privileges

Check SUID/SGID Binaries: List SUID/SGID binaries that might be exploited:

```bash
find / -perm -4000 -o -perm -2000 -type f 2>/dev/null
```

Search for Local Vulnerabilities: Look for misconfigurations or vulnerabilities in the VM's configuration and software.

Privilege Escalation Techniques: Consider techniques like exploiting vulnerable services, leveraging misconfigured permissions, or using known privilege escalation exploits.

6. Final Steps

Obtain the Flag: After gaining root access, locate the flag. It is typically found in /root or /home directories.

Document Your Findings: Ensure all steps taken, tools used, and findings are documented in this README.

## Conclusion

This document outlines the methodology for obtaining root access on the provided VM using network scanning and vulnerability exploitation. It emphasizes the importance of documenting each step and using ethical hacking practices.

# Disclaimer

The methods and tools described here are intended for educational purposes only. Ensure you have explicit permission before attempting any exploit-type activity. Unauthorized access is illegal and unethical.
References

Nmap Documentation
Metasploit Framework
Exploit-DB

github.com/dgthegeek