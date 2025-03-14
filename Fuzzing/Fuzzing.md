**What is Fuzzing?**

Fuzzing is an automated technique used in **penetration testing** and **bug hunting** where a tool or script **sends large amounts of unexpected, malformed, or random data** to a target application to discover **vulnerabilities, crashes, or security flaws**.

 **Goal:** Find **unhandled exceptions, buffer overflows, injections, misconfigurations**, or **security bypasses**.

** Types of Fuzzing**

|**Type**|**Description**|**Example**|
|---|---|---|
|**Web Fuzzing**|Testing for security vulnerabilities in web applications|SQL Injection, LFI, RCE|
|**Network Fuzzing**|Testing network protocols for weaknesses|TCP/UDP malformed packets|
|**Binary Fuzzing**|Searching for vulnerabilities in compiled software|Buffer Overflow, Heap Overflows|
|**API Fuzzing**|Sending unexpected requests to APIs|Fuzzing JSON/XML inputs|
|**File Format Fuzzing**|Testing how an application processes files|PDF/PNG Exploits|

**What is HTTP Header Fuzzing?**

HTTP Header Fuzzing is a technique used to **test how a web server or application processes HTTP headers**. Attackers use it to find **misconfigurations, WAF bypasses, authentication weaknesses, and security vulnerabilities**.

  

 **Example:**

When a server **doesn’t properly validate HTTP headers**, an attacker may:

• **Bypass security protections**

• **Leak sensitive information**

• **Trigger server-side vulnerabilities**

**Common HTTP Headers to Fuzz**

|**Header**|**Attack Surface**|**Example**|
|---|---|---|
|**User-Agent**|Identify WAF bypasses|"Mozilla/5.0 <script>alert(1)</script>"|
|**Referer**|Bypass weak CSRF protection|Referer: evil.com|
|**Host**|Virtual host routing attacks|Host: internal.site.com|
|**X-Forwarded-For**|IP spoofing|X-Forwarded-For: 127.0.0.1|
|**Cookie**|Session manipulation|Cookie: PHPSESSID=1234|
|**Authorization**|Test authentication weaknesses|Authorization: Basic dXNlcjpwYXNz|
|**Accept-Encoding**|Test server-side compression bugs|Accept-Encoding: gzip,deflate,br|

** HTTP Header Fuzzing Tools**

  

✅ **ffuf** – Fast web fuzzer

✅ **Burp Suite Intruder** – Web-based fuzzing

✅ **wfuzz** – Web app fuzzer

✅ **fuzzdb** – Payload repository for fuzzing

✅ **SecLists** – Common wordlists for fuzzing

**🔹 Example: HTTP Header Fuzzing with ffuf**

  

📌 **Fuzzing the User-Agent Header**

```
ffuf -u http://target.com -H "User-Agent: FUZZ" -w /usr/share/seclists/Fuzzing/UserAgents.txt
```

✅ **Why?**

• Identify **security misconfigurations**

• Detect **WAF behavior**

• Test **header-based security controls**

**🔹 Key Takeaways for OSCP**

  

✅ **Fuzzing is critical for discovering vulnerabilities**

✅ **HTTP headers can be manipulated to bypass security controls**

✅ **Use tools like ffuf, Burp Suite, and wfuzz**

✅ **Look for unhandled exceptions, crashes, or interesting server responses**
