# NETWORKWALKS-PRINCE-B083-WK2-CYBERSECURITY-LAB-SETUP[README.md](https://github.com/user-attachments/files/34772676/README.md)
# Penetration Testing Report (Sample)

### Footprinting & Network Scanning Phases

**W2-PM-FINAL | Cybersecurity | Networkwalks**

| Field | Detail |
|---|---|
| **Pentester Name (Cybersecurity Professional)** | Emmanuel Bafi |
| **Program/Batch** | B082-Networkwalks |
| **Date** | 17 August 2026 |
| **Modules completed** | W2-PM1 (Multiple Kali Tools)<br>W2-PM5 (Zenmap Scanning) |
| **Client/Target** | 1. Networkwalks (secured written permission already)<br>2. My own local LAN Network |
| **Permission secured from client?** | Yes |
| **Phases covered** | **Phase 1:** Reconnaissance & Footprinting<br>**Phase 2:** Scanning & Network Discovery<br>**Phase 3-5:** In Progress |

## 1. Liability Disclaimer

I have performed these activities only on the systems & devices where I had secured written permission or the devices/systems that I own myself. All these materials are for education and research purpose only. Do not use anything from here to break the law. The instructor, the authors and Networkwalks are not responsible for what you do with this knowledge. Every action you take is your own responsibility. Misuse can lead to criminal charges, heavy fines, loss of your job and a permanent record. In most countries unauthorised access is a crime even when nothing is damaged.

## 2. Introduction

This report covers footprinting the networkwalks.com domain using multiple Kali Linux tools (W2-PM1) and scanning my own local network with Zenmap (W2-PM5). One module covers the footprinting phase and the other covers the scanning phase, so together they show how an attacker moves from gathering public information to mapping live hosts on a network. It is the Week 2 part of my ongoing internship program at Networkwalks.

All commands were run in Kali Linux (footprinting) and on a Windows PC with Zenmap installed (scanning). Every step below includes the exact command used, the result I observed, a screenshot as evidence, and a short note on why the finding matters from an attacker's point of view.

## 3. Tools Used

The table below lists each tool used in this report and its purpose.

| **Tool**             | **Purpose**                                                    |
|----------------------|----------------------------------------------------------------|
| Kali Linux & Windows | Operating systems used for reconnaissance activities           |
| WHOIS                | Find domain registration details (owner, dates, name servers). |
| whatweb              | Fingerprint web technologies (server, CMS, plugins, IP).       |
| nslookup             | Resolve the domain name to its IP address using DNS.           |
| curl -I              | Read the HTTP response headers of the website.                 |
| wafw00f              | Detect whether a Web Application Firewall protects the site.   |
| dnsrecon             | Enumerate all DNS records (NS, MX, SPF, TXT, SRV).             |
| Zenmap (Nmap GUI)    | Scan a local host to find open ports and services.             |

## 4. Activities Performed

### 4.1 Footprinting & Reconnaissance

I performed reconnaissance against the networkwalks.com domain using six Kali Linux tools: **WHOIS, WhatWeb, Nslookup, Curl, Wafw00f and DNSRecon**. Each tool was used to collect a different type of information about the target.

First, I used **WHOIS** to obtain publicly available domain registration information and identify the domain’s name servers. The results provided information about the domain registration and hosting infrastructure.

I then used WhatWeb to identify technologies used by the website. The results identified WordPress 7.1.4 and WordPress Download Manager 3.3.76, along with other information exposed by the website.

Using **Nslookup**, I resolved the domain name to its IP address. The provided result identified **192.232.216.135**.

I used Curl with the -I option to inspect the HTTP response headers of networkwalks.com. The server returned a 301 redirect to HTTPS and the headers disclosed Server: Apache, the redirect source WordPress - Really Simple Security, a WordPress cache header and a \_\_wpdm_client cookie.

Next, I used **Wafw00f** to determine whether a Web Application Firewall was protecting the website. The result identified **ModSecurity (SpiderLabs)**.

Finally, I used **DNSRecon** to enumerate DNS records. The results provided information relating to name servers, mail servers, SPF/TXT records, service records and DNS software information.

### 4.2 Network Scanning with Zenmap

For the second activity, I used Zenmap to scan my own Windows PC on my local network. The practical required me to identify the target IP address, discover open ports and services, and generate a network topology.

I used the Quick scan profile (nmap -T4 -F) against 192.168.43.184 (hostname DESKTOP-MSJPUIL).

The scan reported the host as up, with 96 filtered TCP ports and four open ports:

- 135/tcp open msrpc

- 139/tcp open netbios-ssn

- 445/tcp open microsoft-ds

- 5357/tcp open wsdapi

Nmap completed the scan of 1 IP address (1 host up) in 2.22 seconds.

After the scan, I opened the Topology tab in Zenmap with the address and hostname options enabled. It shows the scanned host (yellow, meaning 3 to 6 open ports) connected to localhost.

## 5. Risk Analysis / Impact

Based on the information collected during the footprinting and network scanning activities, I identified the following potential risks.

| **\#** | **Risk / Finding**                          | **Evidence / Observation**                                                                    | **Potential Impact**                                                                                            | **Risk Level** |
|--------|---------------------------------------------|-----------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|----------------|
| 1      | Web technology information exposed          | WhatWeb identified WordPress and WordPress Download Manager                                   | Attackers may use exposed technology/version information to identify software requiring further security review | **● Medium**   |
| 2      | Server IP address identifiable              | Nslookup resolved the domain to 192.232.216.135                                               | Provides information about the network location of the web service                                              | **● Low**      |
| 3      | HTTP technical information exposed          | Curl returned HTTP response headers that exposed Apache, WordPress and Really Simple Security | May assist technology fingerprinting and further enumeration                                                    | **● Low**      |
| 4      | WAF technology identifiable                 | Wafw00f identified ModSecurity (SpiderLabs)                                                   | Reveals information about the web application’s security architecture                                           | **● Low**      |
| 5      | DNS infrastructure information exposed      | DNSRecon identified DNS, mail and service-related records                                     | DNS information can help build a broader infrastructure profile                                                 | **● Medium**   |
| 6      | Open Windows network services on local host | Zenmap found ports 135, 139, 445 and 5357 open on 192.168.43.184                              | Open RPC, NetBIOS and SMB ports can help an attacker enumerate the host if they are reachable from the network  | **● Medium**   |

**Risk level key:** ● Critical ● Medium ● Low

The risks above are observations from the footprinting and scanning exercises, not confirmed vulnerabilities.

The practical exercises primarily involved information gathering and host discovery. No exploitation or vulnerability validation was performed as part of these two modules.

Therefore, the presence of information such as a software version, IP address or DNS record does not by itself mean that the system is vulnerable. Further authorized security testing would be required to confirm any actual vulnerability.

## 6. Recommendations

Based on the observations from these activities, I recommend the following security improvements:

1.  **Review publicly exposed technology information**  
    Organizations should regularly review what information about their web technologies, CMS and plugins is publicly visible.

2.  **Keep software updated**  
    CMS platforms, plugins and other web technologies should be regularly updated and reviewed against current security advisories.

3.  **Review HTTP headers**  
    HTTP response headers should be reviewed to determine whether unnecessary technical information is being exposed.

4.  **Review DNS records regularly**  
    DNS records should be checked periodically to ensure that only required information and services are publicly exposed.

5.  **Properly configure and monitor the WAF**  
    Keep the WAF (ModSecurity) enabled and tuned, since it already blocks naive attacks.

6.  **Perform regular internal network discovery**  
    Organizations should periodically scan their own networks to identify active devices.

7.  **Investigate unknown devices**  
    Any unexpected device discovered during network scanning should be investigated and verified.

8.  **Maintain network documentation**  
    Network topology and device information should be documented and updated regularly.

9.  **Perform security testing with authorization**  
    Reconnaissance and scanning should only be performed against systems and networks where appropriate authorization has been provided.

## 7. Conclusion

During Week 2 of my Cybersecurity & Ethical Hacking internship, I completed practical activities covering footprinting, reconnaissance and network scanning.

In the footprinting activity, I used six Kali Linux tools to collect information about the target domain. I learned how WHOIS can provide domain information, WhatWeb can identify web technologies, Nslookup can resolve domain names, Curl can inspect HTTP headers, Wafw00f can identify a WAF, and DNSRecon can provide additional DNS information.

In the network scanning activity, I used Zenmap to scan a host on my local network and identify its open ports and services. I also generated a network topology.

The exercises showed me that information gathering is an important part of cybersecurity. Even before attempting to exploit a system, a security professional can learn a significant amount about an environment by carefully analyzing publicly available information and network responses.

I also learned that technical findings should be documented clearly. A good cybersecurity report should explain what was performed, what was discovered, what the observation means, what risk it may create, and what can be done to reduce that risk.

Finally, I learned that reconnaissance and scanning must always be performed within an authorized scope. These activities were completed as part of the assigned educational cybersecurity lab.

## 8. Evidence Collected

*Screenshots collected as evidence during the activities:*

![whois output](images/image1.png)

![whatweb output](images/image2.png)

![nslookup output](images/image3.png)

![curl output](images/image4.png)

![wafw00f output](images/image5.png)

![Zenmap scan and topology](images/image7.png)

![Zenmap scan and topology](images/image8.png)

---
*End of report*
