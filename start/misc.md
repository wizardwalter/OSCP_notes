# OSCP Quick Reference Guide

## 🔥 Essential Reminders
- **Flags must be submitted before turning off a machine!**
- **Use this command when connecting via SSH:**
  ```bash
  ssh -o "UserKnownHostsFile=/dev/null" -o "StrictHostKeyChecking=no" learner@192.168.50.52

	•	UserKnownHostsFile=/dev/null: Prevents the server host key from being recorded.
	•	StrictHostKeyChecking=no: SSH will not verify the authenticity of the server host key.
	•	⚠️ In the real world, using these options opens up risks for man-in-the-middle (MITM) attacks.

🖥️ Host vs. Guest Machine
	•	Host Machine → The physical system running Windows/macOS (your Mac).
	•	Guest Machine → The Kali Linux VM inside your host system.

🌐 Networking & Machine Access
	•	Use the IP addresses assigned to you via tun0 and the OLP (Offensive Security Lab Portal) to access machines properly.
	•	Machines contain local.txt and/or proof.txt files, which contain randomized hashes used to log each compromise.
	•	Pivoting: Moving from one machine to another on the same network.
	•	Tunneling: Moving into another subnetwork via a secure connection.

🔐 Security Fundamentals
	•	Information Security = Protection of physical and digital information assets.
	•	Cybersecurity = Protection of digital networks, systems, and access control.

🚨 Threats, Vulnerabilities, and Attack Vectors
	•	Threat → Any risk to an asset we want to protect (not all threats are human!).
	•	Vulnerability → A weakness in a system that could be exploited.
	•	Example: The Apache Log4J vulnerability, which allowed remote code execution via Java’s JNDI feature.
	•	Attack Surface → All points of contact that could be exploited.
	•	Attack Vector → A specific vulnerability + exploit combination used by an attacker.

    Zero Trust encapsulates five key elements: Just in Time Access (JITA), which requires access to be validated just before access is granted; Just Enough Access (JEA), which aligns with the traditional concept of least privilege; tokenization and encryption to protect data; dynamic (or adaptive) access control policies to ensure that policy is always fit for purpose; and microsegmentation, to limit access to the appropriate level of granularity.Open Security, a somewhat counter-intuitive principle, states that the security of a system should not depend on its secrecy.Defense in Depth advocates for adding defenses to as many layers of a system as possible, so that if one is bypassed, another may still prevent full infiltration

CIA -
Confidentiality: Can actors who should not have access to the system or information access the system or information?
Integrity: Can the data or the system be modified in some way that is not intended?
Availability: Are the data or the system accessible when and how they are intended to be?
