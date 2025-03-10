SYN scanning is Nmap’s default and most popular technique, often called “stealth” scanning, as it sends SYN packets to target ports without completing the TCP handshake. If a port is open, the target responds with a SYN-ACK, but the scanner does not send the final ACK, making the scan less detectable.

```
sudo nmap -sS 192.168.50.149
```

**sometimes sudo is needed, when in doubt use it.**

```
nmap -sT -A --top-ports=20 192.168.50.1-253 -oG top-port-sweep.txt
```

This **Nmap** command above,  performs a **TCP Connect Scan**(can be longer, uses 3 way handshake instead of -sS which doesnt, aka stealth) (-sT) on the **top 20 most common ports** (--top-ports=20) for all hosts in the **192.168.50.1-253** range.

It also enables **aggressive scanning** (-A), which includes:

✅ OS detection

✅ Service/version detection(-sV)

✅ Script scanning(-sC)

✅ Traceroute
  

The results are **saved in Greppable format** (-oG top-port-sweep.txt), making it easier to filter later. 

# SMB(Server Message Block)

**SMB (Server Message Block)** is a **network file-sharing protocol** used primarily by **Windows** systems to share files, printers, and other resources within a network.
• SMB operates on **port 445** (modern) and **ports 139 & 137** (legacy NetBIOS).

**Key Features of SMB**
• Allows file and folder sharing across a network.
• Supports **authentication** (user credentials) but sometimes allows **anonymous access**.
• Can be used for **enumeration**, **password attacks**, and **exploitation**.

***enumeration of these two services(SMB and NetBios) often goes together***
example nmap scan(notice the ports):

```
nmap -v -p 139,445 -oG smb.txt 192.168.50.1-254
Or
nmap -p 139,445 --script=smb-enum-shares,smb-enum-users 192.168.50.1/24
```

specialized tools for specifically identifying NetBIOS information, such as `nbtscan`
We can use this to query the NetBIOS name service for valid NetBIOS names, specifying the originating UDP port as 137 with the `-r` option

example:
```
sudo nbtscan -r 192.168.50.0/24
```

***NetBIOS names are often very descriptive about the role of the host within the organization***


# SMTP(**Simple Mail Transfer Protocol**)
SMTP is a protocol used to **send** emails between mail servers.
• It operates on:
• **Port 25** (default, often blocked to prevent spam)
• **Port 465** (SMTP over SSL)
• **Port 587** (STARTTLS, modern encryption)
• **Not used for receiving emails** (IMAP & POP3 handle that).

**SMTP Key Features**
• Allows email transmission across networks.
• Uses **mail servers** to route emails to the correct recipient.
• Sometimes supports **VRFY** (verify user) and **EXPN** (expand mailing lists) commands, which can be used for **user enumeration**.

```
nmap -p 25,465,587 --script=smtp-commands <IP>
```
 Lists supported SMTP commands.
```
nmap -p 25 --script=smtp-enum <IP>
```
Retrieves the SMTP banner, which may reveal software version.
```
nmap -p 25 --script=smtp-enum-users <IP>
```
Tries **VRFY** and **EXPN** commands to check if a user exists.
```
nmap -p 25,465,587 --script=smtp-vuln* <IP>
```
Detects misconfigurations and known exploits.

# SNMP(Simple Network Management Protocol)

**SNMP (Simple Network Management Protocol)** is used for **monitoring and managing network devices** such as routers, switches, servers, and printers.
• Operates on:
• **UDP 161** (used for querying SNMP agents)
• **UDP 162** (used for receiving SNMP traps/alerts)
• SNMP allows **reading system information**, including network interfaces, uptime, running processes, and more.

```
nmap -p 161 --script=snmp-info <IP>
```
Retrieves SNMP system information (device name, OS, etc.), is it running??
```
nmap -p 161 --script=snmp-brute <IP>
```
Attempts to brute-force SNMP community strings (default: **public**, **private**).
```
nmap -p 161 --script=snmp-users <IP>
```
Retrieves a list of users from the system.
```
nmap -p 161 --script=snmp-vuln* <IP>
```
detects a list of known smnp vulnerabilities


Another command to use without script
```
sudo nmap -sU --open -p 161 192.168.50.1-254 -oG open-snmp.txt
```



**📌 Nmap Cheat Sheet for OSCP**

**🔹 Basic Scans**

|**Command**|**Description**|
|---|---|
|nmap <target>|Basic scan (default is a SYN scan, if privileged)|
|nmap -sT <target>|TCP Connect scan (used when SYN scan is not possible)|
|nmap -sS <target>|SYN (stealth) scan, doesn’t complete handshake|
|nmap -sU <target>|UDP scan (slower than TCP, used for services like DNS, SNMP)|
|nmap -p- <target>|Scan all **65,535** ports (instead of just the top **1,000**)|
|nmap -F <target>|Fast scan (only scans top 100 ports)|

**🔹 Service & Version Detection**

|**Command**|**Description**|
|---|---|
|nmap -sV <target>|Detects **service versions** running on open ports|
|nmap -sV --version-intensity 9 <target>|Aggressive version detection|
|nmap --allports <target>|Ensures no ports are ignored|
|nmap --top-ports 100 <target>|Scans the top 100 most common ports|

**🔹 OS Detection & Fingerprinting**

|**Command**|**Description**|
|---|---|
|nmap -O <target>|OS detection|
|nmap --osscan-guess <target>|Tries to guess OS when detection is inconclusive|

**🔹 Aggressive Scanning (More Information)**

|**Command**|**Description**|
|---|---|
|nmap -A <target>|Enables OS detection, version detection, script scanning, and traceroute|
|nmap -T4 -A -v <target>|Aggressive scan with **faster timing and verbosity**|

**🔹 Port Scanning Techniques**

|**Command**|**Description**|
|---|---|
|nmap -p 80,443 <target>|Scan specific ports|
|nmap -p 1-1000 <target>|Scan a range of ports|
|nmap -p- <target>|Scan **all 65,535 ports**|
|nmap --top-ports 50 <target>|Scan the **top 50 most used ports**|

**🔹 Bypassing Firewalls & IDS/IPS**

|**Command**|**Description**|
|---|---|
|nmap -f <target>|Fragment packets to bypass firewalls|
|nmap --mtu 16 <target>|Use a specific **MTU (Maximum Transmission Unit)** size|
|nmap -D RND:5 <target>|Decoy scan using 5 random IPs|
|nmap --source-port 53 <target>|Spoof traffic to appear as **DNS queries**|
|nmap -sS -T2 <target>|Slow down scans to avoid detection|

**🔹 Scanning Through Proxychains or VPNs**

|**Command**|**Description**|
|---|---|
|nmap -sT -Pn --proxy socks4://127.0.0.1:9050 <target>|Scan through **Tor (SOCKS proxy)**|
|nmap -sT -Pn -S <spoofed-IP> <target>|Spoof your source IP|
|nmap -e tun0 <target>|Scan through your **VPN interface**|

**🔹 Evading Detection with Timing & Paranoia**

|**Command**|**Description**|
|---|---|
|nmap -T0 <target>|**Paranoid Mode** (slowest, very stealthy)|
|nmap -T1 <target>|**Sneaky Mode** (good for avoiding detection)|
|nmap -T3 <target>|**Normal Mode** (default timing)|
|nmap -T4 <target>|**Aggressive Mode** (faster scanning, but noisier)|
|nmap -T5 <target>|**Insane Mode** (fastest, highly detectable)|

**🔹 Detecting Vulnerabilities & Exploitable Services**

|**Command**|**Description**|
|---|---|
|nmap --script vuln <target>|Run **default vulnerability detection** scripts|
|nmap --script exploit <target>|Run scripts targeting known exploits|
|nmap --script auth <target>|Run authentication bypass scripts|
|nmap --script smb-vuln-ms17-010 -p 445 <target>|Check if the target is vulnerable to **EternalBlue (MS17-010)**|


**🔹 Scanning UDP Ports**

|**Command**|**Description**|
|---|---|
|nmap -sU -p 161,162 <target>|Scan specific UDP ports (e.g., SNMP)|
|nmap -sU -p 53 <target>|Scan **DNS servers** for vulnerabilities|

**🔹 Banner Grabbing & Service Enumeration**

|**Command**|**Description**|
|---|---|
|nmap -sV --script banner <target>|Grab banners to identify running services|
|nmap --script http-title <target>|Get the **title of web pages**|
|nmap --script ssh-hostkey <target>|Get **SSH host keys** for fingerprinting|

**🔹 Detecting Live Hosts on a Network (Host Discovery)**

|**Command**|**Description**|
|---|---|
|nmap -sn <target>/24|Ping scan to detect live hosts|
|nmap -Pn <target>|Scan **without** sending ICMP requests|
|nmap -PS <target>|TCP SYN Ping to detect live hosts|
|nmap -PU <target>|UDP Ping scan|

**🔹 Exporting Scan Results**

|**Command**|**Description**|
|---|---|
|nmap -oN scan.txt <target>|Save results in **normal text format**|
|nmap -oX scan.xml <target>|Save results in **XML format**|
|nmap -oG scan.gnmap <target>|Save results in **Grepable format**|
|nmap -oA scan <target>|Save results in **all formats**|

**📌 Best Nmap Scripts for OSCP**

  
**1️⃣ Service Detection**
```
• **nmap -sV --script=banner <target-ip>**

• 📌 **What it does?** Captures service banners to identify running services.

• 🎯 **Use when:** You want to gather more information about services without aggressive scanning.

• **nmap --script=http-title -p 80,443 <target-ip>**

• 📌 **What it does?** Retrieves the web page title from HTTP services.

• 🎯 **Use when:** Identifying web applications and potential attack surfaces.

• **nmap --script=http-headers -p 80,443 <target-ip>**

• 📌 **What it does?** Shows HTTP response headers (Server info, X-Powered-By, etc.).

• 🎯 **Use when:** Looking for technologies used on a website (e.g., Apache, Nginx, PHP).
```

**2️⃣ Vulnerability Scanning**
```
• **nmap --script=vulners -sV <target-ip>**

• 📌 **What it does?** Checks services against a CVE database for known vulnerabilities.

• 🎯 **Use when:** You suspect outdated or vulnerable software.

• **nmap --script=http-vuln-cve2021-41773 -p 80,443 <target-ip>**

• 📌 **What it does?** Checks if the Apache server is vulnerable to CVE-2021-41773.

• 🎯 **Use when:** You detect Apache running and want to check for a known vulnerability.

• **nmap --script=smb-vuln-ms17-010 -p 445 <target-ip>**

• 📌 **What it does?** Checks if the target is vulnerable to **EternalBlue (MS17-010)**.

• 🎯 **Use when:** You see SMB running on **port 445**, a common Windows vulnerability.
**🔥 Key Takeaways for OSCP**
```

  **3️⃣ SMB (Windows Enumeration)**
```
• **nmap --script=smb-os-discovery -p 445 <target-ip>**

• 📌 **What it does?** Detects Windows version and OS information.

• 🎯 **Use when:** Enumerating **Windows machines**.

• **nmap --script=smb-enum-shares -p 445 <target-ip>**

• 📌 **What it does?** Lists shared folders on the target machine.

• 🎯 **Use when:** Checking for misconfigured or accessible shares.

• **nmap --script=smb-enum-users -p 445 <target-ip>**

• 📌 **What it does?** Retrieves a list of user accounts from the SMB service.

• 🎯 **Use when:** Looking for **valid usernames** for potential brute-force attacks.
```


**4️⃣ Web Application Enumeration**
```
• **nmap --script=http-robots.txt -p 80,443 <target-ip>**

• 📌 **What it does?** Checks for a robots.txt file, which might contain hidden URLs.

• 🎯 **Use when:** Enumerating **hidden directories** on a website.

• **nmap --script=http-enum -p 80,443 <target-ip>**

• 📌 **What it does?** Enumerates common web directories like /admin, /login, etc.

• 🎯 **Use when:** Discovering **hidden paths or admin panels**.

• **nmap --script=http-methods -p 80,443 <target-ip>**

• 📌 **What it does?** Lists allowed HTTP methods (GET, POST, PUT, DELETE, etc.).

• 🎯 **Use when:** Looking for **potentially dangerous HTTP methods** (like PUT for file upload).

• **nmap --script=http-sql-injection -p 80,443 <target-ip>**

• 📌 **What it does?** Tests for **SQL injection** vulnerabilities.

• 🎯 **Use when:** Checking if a web application is vulnerable to **SQLi attacks**.
```

**5️⃣ Brute-Force and Authentication**
```
• **nmap --script=ssh-brute -p 22 <target-ip>**

• 📌 **What it does?** Attempts **SSH login brute-force** using common credentials.

• 🎯 **Use when:** You suspect weak SSH credentials on a target.

• **nmap --script=ftp-anon -p 21 <target-ip>**

• 📌 **What it does?** Checks if **anonymous FTP login** is enabled.

• 🎯 **Use when:** FTP is running and you want to see if you can access files.

• **nmap --script=rdp-enum-encryption -p 3389 <target-ip>**

• 📌 **What it does?** Checks **Remote Desktop Protocol (RDP) security settings**.

• 🎯 **Use when:** You detect **port 3389** (RDP) and want to check for vulnerabilities.
```

**6️⃣ Miscellaneous Enumeration**
```
• **nmap --script=snmp-info -p 161 <target-ip>**

• 📌 **What it does?** Retrieves SNMP system information.

• 🎯 **Use when:** SNMP is open and you want to gather **network device details**.

• **nmap --script=rdp-ms12-020 -p 3389 <target-ip>**

• 📌 **What it does?** Checks for **MS12-020**, a critical RDP vulnerability.

• 🎯 **Use when:** RDP (port 3389) is open, and you suspect an **old Windows server**.
```

✅ Always start with nmap -sS -sV -p- -T4 -A <target> for a thorough initial scan.

✅ Use **aggressive mode (-A)** only when stealth is not a concern.

✅ If scanning is **slow or blocked**, try fragmented packets (-f) or different source ports (--source-port 53).

✅ Use **NSE scripts** to find vulnerabilities faster.

✅ **Export results** (-oA scan_results) to reference later.
