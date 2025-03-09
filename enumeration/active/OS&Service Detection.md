If you don’t specify **OS detection** in your Nmap scan, you can still infer the **operating system** and **services running** based on several factors. Here’s how:

**1️⃣ Look at the Open Ports**

Certain ports are **strong indicators** of an OS:

|**Port**|**Common OS**|
|---|---|
|**135, 139, 445**|Windows (SMB, RPC)|
|**22**|Linux/macOS (SSH)|
|**23**|Legacy systems (Telnet, often old Linux/Unix)|
|**25, 110, 143**|Mail servers (SMTP, POP3, IMAP - usually Linux)|
|**3389**|Windows (RDP)|
|**53**|DNS servers (Linux or Windows)|

**2️⃣ Look at Service Versions**

• Use nmap -sV to **identify service versions**, which often hint at the OS.

• Example output:

```
22/tcp open  ssh  OpenSSH 7.9p1 Debian 10+deb10u2
135/tcp open  msrpc Microsoft Windows RPC
```

• **Debian 10 → Linux**

• **Microsoft RPC → Windows**

**3️⃣ Check HTTP Headers (Web Servers)**

• If a web server is running, check for a **banner**:

```
HTTP/1.1 200 OK
Server: Apache/2.4.41 (Ubuntu)
```

• **Apache 2.4.41 → Likely Linux (Ubuntu)**

• **IIS → Windows Server**

```
nmap --script=http-headers <IP> 
```
• Use to extract this info.

**4️⃣ TTL (Time to Live) in Ping Response**

• TTL values **help guess the OS** (if ICMP is allowed):

|**TTL Value**|**OS Type**|
|---|---|
|**~64**|Linux/macOS|
|**~128**|Windows|
|**~255**|Cisco devices, some network appliances|

• Run:

```
ping -c 1 <IP>
```

If TTL = 127-128, it’s **Windows**

If TTL = 63-64, it’s **Linux**

**5️⃣ SMB (Windows) vs. NFS (Linux)**

• **SMB (135, 139, 445)** → Windows

• **NFS (2049)** → Linux/Unix

Check with:

```
nmap -p 139,445 --script smb-os-discovery <IP>
```

**6️⃣ Reverse DNS Lookup**

• Hostnames sometimes **indicate the OS**:

```
nslookup <IP>
host <IP>
```

Example:

```
192.168.1.5 → WIN-SERVER01.corp.local  (Windows)
10.0.0.10 → mail.linuxserver.com  (Linux)
```

**7️⃣ MAC Address Vendor**

• Some MAC addresses suggest OS types (if detected by -O or -sn):

```
nmap -sn <IP>
```

Example:

```
00:50:56 → VMware (Virtual Machine)
08:00:27 → VirtualBox (Linux VM)
```

**🛠️ Takeaway**

  

Even **without** -O (OS detection), you can **infer** the OS using:

✔ **Port numbers**

✔ **Service banners (-sV)**

✔ **TTL values**

✔ **Protocol usage (SMB, NFS, RDP, SSH, HTTP headers, etc.)**

✔ **Reverse DNS and MAC addresses**
