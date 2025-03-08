# 🖥️ DNS Recon & Enumeration

## 📌 **What is DNS?**
The **Domain Name System (DNS)** is a distributed database that translates domain names into IP addresses.

## 🔥 **Common DNS Record Types**
| Record | Purpose |
|--------|---------|
| **A** | Maps a hostname to an **IPv4 address** |
| **AAAA** | Maps a hostname to an **IPv6 address** |
| **NS** | Nameserver record (authoritative DNS server) |
| **MX** | Mail Exchange (email servers for a domain) |
| **CNAME** | Alias for another domain (e.g., `www.example.com → example.com`) |
| **PTR** | Reverse lookup (resolves IP → hostname) |
| **TXT** | Stores arbitrary text (often used for **SPF, DKIM, and domain verification**) |

---

## 🔍 **DNS Enumeration Techniques**
### **1️⃣ Query a Specific DNS Record**
```bash
nslookup -type=MX megacorpone.com
dig megacorpone.com MX
```
### **2️⃣ Perform a Full DNS Lookup**
```bash
dig megacorpone.com ANY
```

### **3️⃣ Reverse DNS Lookup (Find Hostname from IP)**
```bash
nslookup 192.168.1.1
```
### **4️⃣ Find Subdomains with Passive Recon**
```bash
host -t ns megacorpone.com
```
### **5️⃣ Use dnsrecon for Automated Enumeration**
```bash
dnsrecon -d megacorpone.com -t axfr
OR
dnsrecon -d megacorpone.com -D ~/list.txt -t brt
```
-d = domain name
-D = file name containing potential subdomain strings
-t = type of enumeration to perform, in this case brt for brute force.
### **6️⃣ Bruteforce Subdomains (Active Recon)**
```bash
sublist3r -d megacorpone.com
```
### **7️⃣ DNS transder attempt (zone transfer)**
```bash
dig AXFR @ns1.megacorpone.com megacorpone.com
```
A Zone Transfer (AXFR) is a mechanism used to replicate DNS records between authoritative primary and secondary DNS servers.
However, misconfigured DNS servers may allow unauthorized zone transfers, which can leak sensitive information about a target’s infrastructure.

### manual bruteforce - create list.txt of subdomains you find fitting.
```bash
for ip in $(cat list.txt); do host $ip.megacorpone.com; done
```
with ips found can perform

```bash
for ip in $(seq 200 254); do host 51.222.169.$ip; done | grep -v "not found"
```
