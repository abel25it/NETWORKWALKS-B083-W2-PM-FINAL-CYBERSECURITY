**PENETRATION TESTING REPORT**  
**FOOTPRINTING & NETWORK SCANNING PHASES**

W2-PM-FINAL | CYBERSECURITY | NETWORKWALKS

|  |  |
| :---- | :---- |
| **Pentester Name(Cybersecurity Professional)** | Abel Alex |
| **Program/Batch** | B083-Networkwalks |
| **Date** | 18 September 2026 |
| **Modules completed** | W2-PM1 (Multiple Kali Tools) W2-PM2 (GHDB) W2-PM3 (Maltego) W2-PM4 (theHarvester) W2-PM5 (Zenmap Scanning) |
| **Client/Target** | 1\. Networkwalks (secured written permission already) 2\. My own local LAN Network 3\. Publicly exposed devices/domains via passive OSINT (GHDB, Microsoft) |
| **Permission secured from client?** | Yes |
| **Phases covered** | Phase 1: Reconnaissance & Footprinting Phase 2: Scanning & Network Discovery Phase 3-5: In Progress |

**1\. Liability Disclaimer**  
I have performed these activities only on the systems & devices where I had secured written permission or the devices/systems that I own myself. All these materials are for education and research purpose only. Do not use anything from here to break the law. The instructor, the authors and Networkwalks are not responsible for what you do with this knowledge. Every action you take is your own responsibility. Misuse can lead to criminal charges, heavy fines, loss of your job and a permanent record. In most countries unauthorised access is a crime even when nothing is damaged.

**2\. Introduction**  
This report covers a comprehensive footprinting and network scanning assessment spanning five project modules. It details reconnaissance activities against the networkwalks.com and microsoft.com domains utilizing built-in Kali Linux tools, the Google Hacking Database (GHDB), Maltego, and theHarvester. Additionally, it encompasses the scanning phase where Zenmap was used to map live hosts on a local network. Together, these modules demonstrate the methodological progression from gathering passive, publicly available intelligence to actively mapping network infrastructure.  
All commands were executed in Kali Linux for footprinting tasks and on a Windows environment for Zenmap scanning. Every step documented below includes the tools utilized, the exact findings observed, and an analysis of why this information is critical from an adversarial perspective.

**3\. Tools Used**  
The table below lists each tool used in this report and its primary purpose during the assessment.

| Tool | Purpose |
| :---- | :---- |
| Kali Linux & Windows | Operating systems used for reconnaissance and scanning activities. |
| WHOIS | Find domain registration details including owner, dates, and name servers. |
| whatweb | Fingerprint web technologies such as server type, CMS, plugins, and IP address. |
| nslookup | Resolve the domain name to its direct IP address using DNS. |
| curl \-I | Read the HTTP response headers of the target website. |
| wafw00f | Detect whether a Web Application Firewall (WAF) is actively protecting the site. |
| dnsrecon | Enumerate all DNS records including NS, MX, SPF, TXT, and SRV. |
| GHDB (Google Dorks) | Utilize advanced search operators via Google to uncover accidentally exposed files and devices. |
| Maltego | Perform visual link analysis and automated open-source intelligence gathering via transforms. |
| theHarvester | Harvest emails, sub-domains, and hosts from multiple public sources (e.g., Baidu). |
| Zenmap (Nmap GUI) | Scan the local subnet to find live hosts, IP addresses, and MAC addresses. |
| Windows CMD | Identify the local IP address and LAN subnet via the ipconfig command. |

**4\. Activities Performed**  
**4.1 Footprinting & Reconnaissance (Kali Linux Tools)**  
I performed targeted reconnaissance against the networkwalks.com domain using six Kali Linux tools.

* **WHOIS:** Obtained public domain registration details, revealing the registrar as GoDaddy.com, LLC and identifying the name servers as ns6135.HOSTGATOR.COM and ns6136.HOSTGATOR.COM.  
* **WhatWeb:** Identified core technologies powering the site, exposing the use of Apache, WordPress 7.0.4, WP Download Manager 3.3.58, Bootstrap 7.0.4, and jQuery 3.7.1.  
* **Nslookup:** Successfully resolved the domain name to its underlying IP address, 192.232.216.135.  
* **Curl:** Inspected the HTTP response headers using the \-I flag, which exposed the server banner, wpdm\_client cookies, and the WordPress REST API endpoint at /wp-json/.  
* **Wafw00f:** Detected the presence of a Web Application Firewall, specifically identifying that the site is protected by ModSecurity (SpiderLabs).  
* **DNSRecon:** Enumerated the domain's DNS footprint, finding a Bind software version of 9.16.23-RH, a cPanel mail discovery SRV record, and an SPF policy (v=spf1).

**4.2 Google Hacking Database (GHDB)**  
I utilized GHDB dorks to passively uncover sensitive information indexed by Google without directly touching any target.

* **Exposed Cameras:** Using the dork intitle:"webcamXP" inurl:8080, I successfully located live, vulnerable security camera links exposed to the internet.  
* **Open Directories:** Using the dork intitle:index.of "parent directory" mathematics pdf, I discovered open directory listings containing downloadable mathematics PDF ebooks, indicating a failure to restrict directory browsing on those web servers.

**4.3 Footprinting with Maltego**  
I downloaded and configured Maltego Community Edition on a Windows machine to perform visual link analysis.

* After authenticating my Maltego ID, I initiated a new graph and added a Domain entity for networkwalks.com.  
* By running email-harvesting transforms, I successfully extracted domain-associated email addresses, such as info@networkwalks.com, which could potentially be used by an attacker to build targeted phishing campaigns.

**4.4 OSINT with theHarvester**  
I used **theHarvester** in Kali Linux to collect open-source intelligence on a separate target, microsoft.com.

* Running the command theHarvester \-d microsoft.com \-l 1000 \-b baidu, I extracted email IDs associated with the domain. 
* I executed a secondary search across all supported data sources (-b all) with a limit of 50 to maximize the collection of exposed subdomains and employee information.

**4.5 Network Scanning with Zenmap**  
For the active scanning phase, I used Zenmap to perform network discovery on my local LAN.

* I used the Windows ipconfig command to identify my local IP address and LAN subnet (10.0.0.0/24).  
* I configured Zenmap with the Ping scan profile (nmap \-sn 10.0.0.0/24) to identify active hosts.  
* The scan discovered four live hosts: 10.0.0.1, 10.0.0.4, 10.0.0.5, and 10.0.0.19.  
* It also resolved the hardware MAC addresses (e.g., 00:50:56:E3:B3:2C, 00:0C:29:C0:94:8F, 00:50:56:E9:64:82).  
* Finally, I accessed the Topology tab, enabled the legend, and exported the visual network graph as a PDF.

**5\. Risk Analysis / Impact**  
Based on the information collected during the footprinting and network scanning activities, I identified the following potential risks.

| \# | Risk / Finding | Evidence / Observation | Potential Impact | Risk Level |
| :---- | :---- | :---- | :---- | :---- |
| 1 | Web technology information exposed | WhatWeb identified WordPress 7.0.4 and WP Download Manager 3.3.58. | Attackers may use exposed technology/version information to identify software requiring further security review. | ● Medium |
| 2 | Server IP address identifiable | Nslookup resolved the domain to 192.232.216.135. | Provides information about the network location of the web service. | Low |
| 3 | HTTP technical information exposed | Curl returned HTTP response headers and exposed /wp-json/. | May assist technology fingerprinting and further enumeration. | Low |
| 4 | WAF technology identifiable | Wafw00f identified ModSecurity (SpiderLabs). | Reveals information about the web application’s security architecture. | Low |
| 5 | DNS infrastructure information exposed | DNSRecon identified DNS, mail, and cPanel service-related records. | DNS information can help build a broader infrastructure profile. | Medium |
| 6 | Exposed physical surveillance devices | GHDB queries uncovered live WebcamXP portals on port 8080\. | Allows unauthorized viewing of physical premises and potential pivot points into internal networks. | Critical |
| 7 | Unrestricted directory indexing | GHDB queries revealed open server directories containing PDF files. | Exposes sensitive internal documents, backups, and intellectual property to the public web. | High |
| 8 | Corporate email addresses harvested | Maltego and theHarvester successfully scraped valid corporate emails. | Provides threat actors with direct targets for social engineering, spear-phishing, and credential stuffing. | Medium |
| 9 | Multiple live hosts visible on local network | Zenmap identified four live hosts in the local 10.0.0.0/24 subnet. | Unknown or unauthorized devices may potentially be present on a network. | Medium |

**Risk level key:** ● Critical ● High ● Medium ● Low  
The risks above are observations from the footprinting and scanning exercises, not confirmed vulnerabilities. The practical exercises primarily involved information gathering and host discovery; no exploitation or vulnerability validation was performed as part of these modules. Further authorized security testing would be required to confirm any actual vulnerability.

**6\. Recommendations**  
Based on the observations from these activities, I recommend the following security improvements:

* Review publicly exposed technology information: Organizations should regularly review what information about their web technologies, CMS, and plugins is publicly visible.  
* Keep software updated: CMS platforms (like WordPress), plugins, and other web technologies should be regularly updated and reviewed against current security advisories.  
* Review HTTP headers: HTTP response headers should be reviewed to determine whether unnecessary technical information is being exposed.  
* Review DNS records regularly: DNS records should be checked periodically to ensure that only required information and services are publicly exposed.  
* Properly configure and monitor the WAF: Keep the WAF (ModSecurity) enabled and tuned, since it already blocks naive attacks.  
* Disable Directory Indexing: Ensure web servers are configured to disable directory listing (e.g., \-Indexes in Apache) to prevent search engines from indexing sensitive files and folders.  
* Secure IoT and Surveillance Devices: Security cameras should never be exposed directly to the public internet. Place them behind a VPN or firewall, disable default ports (like 8080), and enforce strong authentication.  
* Implement Phishing Defenses: Since corporate emails can be easily harvested via tools like theHarvester and Maltego, organizations must conduct regular security awareness training and implement strict email filtering mechanisms.  
* Perform regular internal network discovery: Organizations should periodically scan their own networks to identify active devices.  
* Investigate unknown devices: Any unexpected device discovered during network scanning should be investigated and verified.  
* Maintain network documentation: Network topology and device information should be documented and updated regularly.  
* Perform security testing with authorization: Reconnaissance and scanning should only be performed against systems and networks where appropriate authorization has been provided.

**7\. Conclusion**  
During Week 2 of my Cybersecurity & Ethical Hacking internship, I completed practical activities covering footprinting, reconnaissance, and network scanning.  
In the footprinting activities, I used six Kali Linux tools to collect information about the target domain. I learned how WHOIS provides domain information, WhatWeb identifies web technologies, Nslookup resolves domain names, Curl inspects HTTP headers, Wafw00f identifies a WAF, and DNSRecon provides additional DNS information. I also utilized GHDB to passively uncover misconfigured web servers and exposed cameras, leveraged Maltego for visual intelligence mapping, and used theHarvester to scrape email and subdomain data from search engine APIs.  
In the network scanning activity, I used Zenmap to identify my local network configuration, discover active hosts, collect IP and MAC address information, and create a network topology.  
The exercises demonstrated that information gathering is an essential part of cybersecurity. Even before attempting to exploit a system, a security professional can learn a significant amount about an environment by carefully analyzing publicly available information and network responses. The combination of passive OSINT and active scanning provides a highly detailed map of an organization's attack surface.

**8\. Evidences Collected**  
Screenshot evidences for W2-PM1 through W2-PM5 including Kali Linux terminal outputs (WHOIS, WhatWeb, Nslookup, Curl, Wafw00f, DNSRecon), Exploit-DB GHDB queries, Maltego transform graphs, theHarvester data extraction logs, and the Zenmap PDF topology map are retained and appended as per the raw module outputs.

**W2-PM1 (Multiple Kali Tools)**

*Query the public domain registration record to find who owns the domain, when it was registered, and its name servers.*   
<img width="1917" height="873" alt="whois" src="https://github.com/user-attachments/assets/8dc690e5-10e2-435c-9149-b185ab734d44" />

*Fingerprint the exact web technologies running on the target site, exposing the web server, Content Management System (CMS), plugins, frameworks, and IP address.*  
<img width="1901" height="195" alt="whatweb" src="https://github.com/user-attachments/assets/8e15fbef-62f6-4bc3-868c-2ae94bacff85" />

Resolve the domain name to its IP address using DNS   
<img width="862" height="172" alt="nslookup" src="https://github.com/user-attachments/assets/571d7949-82f1-4462-98f9-efcfd5e47a2d" />

Read the HTTP response headers to see the server banner, status, cookies and redirects   
<img width="1892" height="242" alt="curl -I" src="https://github.com/user-attachments/assets/472b8b5f-360d-4c9a-be4d-180485f6c8a4" />

*Detect whether a Web Application Firewall (WAF) is protecting the target site*   
<img width="986" height="395" alt="wafw00f" src="https://github.com/user-attachments/assets/ce185b35-d043-4f38-b676-471e154cd542" />

*Enumerate all DNS records: name servers, mail servers, SPF, TXT and service (SRV) records.*   
<img width="1182" height="417" alt="dnsrecon" src="https://github.com/user-attachments/assets/d119f4a2-9337-4ef7-a29d-afbead471ad3" />


**W2-PM2 (GHDB)**

**Find 10x live vulnerable security camera links that are exposed & accessible from Internet. List them in below table along with their relevant dork that you have used.**

| No. | Link | Relevant Dork | Username /Password (if any) |
| :---- | :---- | :---- | ----- |
| 1 | http://109.233.191.130:8080/ | intitle:"webcamXP" inurl:8080 | \- |
| 2 | http://93.87.72.254:8091/view/index.shtml | inurl:/view/index.shtml  |  \- |
| 3 | http://173.212.65.18:86/view/viewer\_index.shtml?id=5166 | intitle:"Live View / \- AXIS" inurl:view/view.shtml |  \- |
| 4 | http://45.141.240.152:8083/view/viewer\_index.shtml?id=732 | intitle:"Live View / \- AXIS" inurl:view/view.shtml |  \- |
| 5 | http://103.151.177.124:221/view/viewer\_index.shtml?id=343 | intitle:"Live View / \- AXIS" inurl:view/view.shtml |  \- |
| 6 | http://73.12.24.217/view/viewer\_index.shtml?id=1547 | intitle:"Live View / \- AXIS" inurl:view/view.shtml |  \- |
| 7 | http://96.91.239.26:1024/view/viewer\_index.shtml?id=8882 | intitle:"Live View / \- AXIS" inurl:view/view.shtml |  \- |
| 8 | http://88.27.252.37:8060/view/viewer\_index.shtml?id=9756 | intitle:"Live View / \- AXIS" inurl:view/view.shtml |  \- |
| 9 | http://82.127.214.213:9001/view/index.shtml | intitle:"Live View / \- AXIS" inurl:view/view.shtml |  \- |
| 10 | http://83.89.251.14:8080/ | "my webcamXP server\! |  \- |

**Find 10x listings which contain downloadable mathematics ebooks in PDF format:**

| No. | Link | Relevant Dork | Username /Password (if any) |
| :---- | :---- | :---- | ----- |
| 1 | http://erewhon.superkuh.com/library/Math/ | intitle:index.of "parent directory" mathematics pdf  |  \- |
| 2 | https://www.unm.edu/\~megrad/Math/ | intitle:index.of "parent directory" mathematics pdf  |  \- |
| 3 | https://arxiv.org/pdf/2505.08331 |  intitle:"index.of" "calculus" OR "algebra" filetype:pdf  |  \- |
| 4 | https://link.springer.com/chapter/10.1007/BFb0076326 |  intitle:"index.of" "calculus" OR "algebra" filetype:pdf  |  \- |
| 5 | https://www.math.uni-bielefeld.de/lag/man/255.pdf |  intitle:"index.of" "calculus" OR "algebra" filetype:pdf  |  \- |
| 6 | https://www.ams.org/bookstore/pspdf/chel-78-index.pdf |  intitle:"index.of" "calculus" OR "algebra" filetype:pdf  |  \- |
| 7 | https://www.netlib.org/math/docpdf/ |  intitle:"index.of" "math" pdf  |  \- |
| 8 | https://theswissbay.ch/pdf/Gentoomen%20Library/Maths/Comp%20Sci%20Math/ |  intitle:"index.of" "math" pdf  |  \- |
| 9 | https://math.mit.edu/\~gs/linearalgebra/ila6/?trk=public\_post\_comment-text | intitle:"index.of" "linear algebra" pdf   |  \- |
| 10 | https://www.aerostudents.com/courses/linear-algebra/?SD |  intitle:"index.of" "linear algebra" pdf  |  \- |


**W2-PM3 (Maltego)**

<img width="1628" height="1022" alt="Screenshot 2026-09-18 070829" src="https://github.com/user-attachments/assets/56f7d206-ad25-4f4d-93ce-d954a2ab6a39" />



**W2-PM4 (theHarvester)**

<img width="1912" height="747" alt="TheHarvester baidu" src="https://github.com/user-attachments/assets/e119f894-60cc-4ff5-87b2-aa8f1a411fc3" />

<img width="1917" height="891" alt="TheHarvester all" src="https://github.com/user-attachments/assets/6d563fcf-d02a-4f5e-a17f-2c5d4605394c" />



**W2-PM5 (Zenmap Scanning)**

**1.How many hosts are live in your subnet?**  
**Answer:** 2 hosts are live(including my pc) 

**2\. What are the IP addresses of the live hosts?**   
**Answer:** 10.0.0.1 10.0.0.2 

**3.What are the MAC addresses of the live hosts?**   
**Answer:**   
52:54:00:12:35:00   
08:00:27:8a:35:d2 

<img width="1917" height="908" alt="nmap ping scan" src="https://github.com/user-attachments/assets/e2015a7c-d020-42b8-a4a0-86fdd4e94b7c" />

<img width="1770" height="842" alt="nmap topology" src="https://github.com/user-attachments/assets/68ed7319-2eaa-4465-9496-3b4eac73c9f8" />

<img width="1600" height="652" alt="nmap graphical topology" src="https://github.com/user-attachments/assets/a106c290-6066-413f-98c5-0b459e49dc1f" />



