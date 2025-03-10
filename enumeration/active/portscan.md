
# 🔍 TCP & Netcat Scanning

## **📌 TCP Handshake Process**
1. A host sends a **TCP SYN** packet to a server on a **destination port**.
2. If the **port is open**, the server responds with a **SYN-ACK** packet.
3. The client then sends an **ACK** packet to complete the **three-way handshake**.
4. If the handshake completes successfully, the **port is considered open**.

---

## **📌 Netcat for Port Scanning**
⚠️ **Netcat (nc) is NOT a traditional port scanner**, but it can still be used for scanning.

### **🔹 Example: TCP Port Scan**
We can demonstrate this by running a **TCP Netcat scan** on ports **3388-3390**.  
The command uses:
- `-w 1` → Set a **connection timeout** of 1 second.
- `-z` → **Zero-I/O mode**, meaning Netcat sends no actual data (only checking if ports are open).
- `-nvv` → No DNS resolution, **verbose output**.


```bash
nc -nvv -w 1 -z 192.168.50.152 3388-3390
````

**📌 UDP Scanning with Netcat**

To perform a **UDP scan**, add the -u flag:

```
nc -u -nvv -w 1 -z 192.168.50.152 53
```

🎯 This scans **UDP port 53** (commonly used for DNS).

✅ **Key Takeaways:**

• TCP uses a **three-way handshake** to determine open ports.

please see nmap as this is the best port scanner imo

### SMB:
##### enum4linux

it is a standalone Linux tool used for enumerating information from Windows machines, specifically those running **SMB (Server Message Block) services**. It is commonly used in penetration testing and CTF challenges like OSCP and Hack The Box to gather information about a Windows machine, such as:
• User accounts
• Shares
• Policies
• Group memberships
• Password policies
• Domain information

It is based on smbclient, rpcclient, and net (part of Samba), and it’s often used to check for anonymous access or weak configurations in SMB.
**Common Usage:**
```
enum4linux -a 10.10.10.5
```

to get deeper, once you know you can login with above(will give u credentials or "" for username and pass if none), run this
blank user and pass:
```
smbclient -L //192.168.126.13 -N 
```

if creds(get share from enum4linux):
```
smbclient //<target-ip>/<share-name> -U alfred
```
**Your Attack Mindset Should Be:**
✅ **Find SMB shares**
✅ **Check if anonymous access is allowed**
✅ **Look inside for sensitive files**
✅ **Extract credentials, scripts, logs, backups, configs, etc.**

#### SMTP Enumeration
port 25
A VRFY request asks the server to verify an email address, while EXPN asks the server for the membership of a mailing list.
example scans on port 25

to attack get ip that you know is running something on port 25, then:

```
nc -nv 192.168.50.8 25
```


python script that can help exploit smtp if a know username is available:
```
#!/usr/bin/python

import socket
import sys

if len(sys.argv) != 3:
        print("Usage: vrfy.py <username> <target_ip>")
        sys.exit(0)

# Create a Socket
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Connect to the Server
ip = sys.argv[2]
connect = s.connect((ip,25))

# Receive the banner
banner = s.recv(1024)

print(banner)

# VRFY a user
user = (sys.argv[1]).encode()
s.send(b'VRFY ' + user + b'\r\n')
result = s.recv(1024)

print(result)

# Close the socket
s.close()
```


### SNMP

SNMP is based on ***UDP***, a simple, stateless protocol, and is therefore susceptible to IP spoofing and replay attacks

The **SNMP MIB Tree (Management Information Base)** is a structured database used for **network management**. It organizes information in a hierarchical tree format, where **branches** represent different organizations or network functions, and **leaves** correspond to specific variables that store data. These variables can be queried by external users to retrieve system information, monitor network performance, or manage devices remotely.

Alternatively, we can use a tool such as [_onesixtyone_](http://www.phreedom.org/software/onesixtyone/), which will attempt a brute force attack against a list of IP addresses. First, we must build text files containing community strings and the IP addresses we wish to scan.

```
onesixtyone -c community -i ips
```

We can probe and query SNMP values using a tool such as _snmpwalk_, provided we know the SNMP read-only community string, which in most cases is "public".
 `-c` option to specify the community string, and `-v` to specify the SNMP version number, as well as the `-t 10` option to increase the timeout period to 10 seconds

```
snmpwalk -c public -v1 -t 10 192.168.50.151
```

next command uses an OID at the end to get all windows users(must obv be a windows machine):

```
snmpwalk -c public -v1 192.168.50.151 1.3.6.1.4.1.77.1.2.25
```

next will get all processes running:

```
snmpwalk -c public -v1 192.168.50.151 1.3.6.1.2.1.25.4.2.1.2
```

next all software installed on machine:
```
snmpwalk -c public -v1 192.168.50.151 1.3.6.1.2.1.25.6.3.1.2
```

next get current TCP listening ports:
```
snmpwalk -c public -v1 192.168.50.151 1.3.6.1.2.1.6.13.1.3
```

# Windows

### PortScan

The [_Test-NetConnection_](https://docs.microsoft.com/en-us/powershell/module/nettcpip/test-netconnection?view=windowsserver2022-ps) function checks if an IP responds to ICMP and whether a specified TCP port on the target host is open.

```
Test-NetConnection -Port 445 192.168.50.151
```

This **PowerShell** command iterates over ports **1 to 1024** and attempts to establish a **TCP connection** to each port on the target **192.168.50.151**. If the connection is successful, it prints a message indicating that the port is open.

```
1..1024 | % {echo ((New-Object Net.Sockets.TcpClient).Connect("192.168.50.151", $_)) "TCP port $_ is open"} 2>$null
```


### SMB

One useful tool for enumerating SMB shares within Windows environments is `net view`. It lists domains, resources, and computers belonging to a given host.
By providing the `/all` keyword, we can list the administrative shares ending with the dollar sign.

example:
```
net view \\dc01 /all
```

#### SMTP
port 25
can give us info but need to install TelNetClient(which need admin priv or grab from another development machine) for full interactions.
```
Test-NetConnection -Port 25 192.168.50.8
```

telnet:
```
telnet 192.168.50.8 25
```
