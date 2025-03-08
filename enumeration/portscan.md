
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


# Windows

The [_Test-NetConnection_](https://docs.microsoft.com/en-us/powershell/module/nettcpip/test-netconnection?view=windowsserver2022-ps) function checks if an IP responds to ICMP and whether a specified TCP port on the target host is open.

```
Test-NetConnection -Port 445 192.168.50.151
```

This **PowerShell** command iterates over ports **1 to 1024** and attempts to establish a **TCP connection** to each port on the target **192.168.50.151**. If the connection is successful, it prints a message indicating that the port is open.

```
1..1024 | % {echo ((New-Object Net.Sockets.TcpClient).Connect("192.168.50.151", $_)) "TCP port $_ is open"} 2>$null
```

