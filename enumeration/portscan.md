
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
