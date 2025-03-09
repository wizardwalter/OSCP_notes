🔎 Information Gathering & OSINT Techniques  

## 📌 Google Dorking (Advanced Search Queries)  
Use **Google dorks** to uncover hidden information that is not directly accessible through a website's UI.  

https://www.exploit-db.com/google-hacking-database

- **Find specific file types on a domain:**  
  site:megacorpone.com filetype:php OR filetype:txt OR filetype:py

	•	This searches for .php, .txt, and .py files within the megacorpone.com domain.
	•	Exclude specific items to narrow search results:

site:megacorpone.com -inurl:admin

	•	This excludes results containing “admin” in the URL.

## 🌍 Netcraft (Deprecated, but Useful for Recon)
	•	Netcraft provides web reconnaissance tools, identifying:
	•	Web technologies & hosting infrastructure.
	•	Sites sharing the same IP block.
	•	Subdomains & network structure insights.
	•	🔗 Visit Netcraft

## 🏗 GitHub Recon

GitHub contains code, configurations, and potentially leaked secrets:
	•	Search for repositories owned by a company:

owner:megacorpone

	•	Find users, tech stacks, and accidental credential leaks:

owner:megacorpone path:users

	•	Use advanced search operators to find exposed credentials:

owner:megacorpone password OR api_key OR secret


🚨 Tip: Automate GitHub recon with tools like GitHub-Dorks or Gitleaks.

## 🔥 Shodan - The “Google” for Devices & Servers

Shodan is a search engine for internet-connected devices—ideal for discovering open ports, services, and vulnerabilities.
	•	What Shodan can reveal:
	•	Open ports & running services.
	•	Banner information (software versions & potential exploits).
	•	Publicly exposed databases, IoT devices, and misconfigurations.
	•	Basic Search Example:

hostname:megacorpone.com


	•	Find open RDP servers:

port:3389


	•	Detect exposed MySQL databases:

port:3306 product:mysql



🚨 Tip: Always cross-check Shodan findings with nmap scans.

## 🍄 https://securityheaders.com/ 

will analyze HTTP response headers and provide basic analysis of the target site's security posture. We can use this to get an idea of an organization's coding and security practices based on the results.

## 👀 https://www.ssllabs.com/ssltest/

analyzes a server's SSL/TLS configuration and compares it against current best practices

identify some SSL/TLS related vulnerabilities, such as Poodle or Heartbleed

🎯 Bonus: Other OSINT Resources
	•	Censys.io - Alternative to Shodan for scanning public-facing assets.
	•	Hunter.io - Find company email addresses based on domains.
	•	theHarvester - Automate email, subdomain, and user enumeration.

🔹 Tip: Combine multiple OSINT tools for comprehensive reconnaissance before launching attacks. 🚀
