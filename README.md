# Week 2 Cybersecurity & Ethical Hacking Project

**Student:** Shiv Das  
**Batch:** BO83  
**Program:** Networkwalks Internship  
**Date:** 18 September 2026  

## Project Overview

This project documents Week 2 practical activities covering authorized reconnaissance, footprinting, open-source intelligence and local-network host discovery. The work was completed using Kali Linux, Maltego, theHarvester and Nmap CLI.

> **Ethics statement:** Testing was limited to the explicitly authorized `networkwalks.com` target and my own virtual LAN. No exploitation, password testing, authentication bypass or destructive activity was performed. Public indexing was not treated as authorization.

## Modules Completed

- PM1 — Footprinting with multiple Kali tools
- PM2 — GHDB and search-based OSINT
- PM3 — Footprinting with Maltego
- PM4 — Footprinting with theHarvester
- PM5 — Network discovery with Nmap CLI instead of Zenmap

---

## PM1 — Footprinting with Multiple Kali Tools

Target: `networkwalks.com`

| Tool | Command | Key observation |
|---|---|---|
| WHOIS | `whois networkwalks.com` | GoDaddy registrar, privacy-protected registration and HostGator nameservers were identified. |
| WhatWeb | `whatweb networkwalks.com` | Apache, WordPress 7.1.1, WP Download Manager 3.3.58 and other web technologies were detected. |
| Nslookup | `nslookup networkwalks.com` | The domain resolved to `192.232.216.135`. |
| Curl | `curl -I https://networkwalks.com` | HTTP/2 200, Apache and WordPress REST API links were visible in the headers. |
| Wafw00f | `wafw00f networkwalks.com` | ModSecurity (SpiderLabs) WAF was identified. |
| DNSRecon | `dnsrecon -d networkwalks.com` | SOA, NS, MX, A, TXT/SPF and SRV information was enumerated. |

These results are reconnaissance observations and are not proof of exploitable vulnerabilities.

### PM1 Evidence

#### WHOIS
![WHOIS registration summary](evidence/screenshots/pm1-whois-1.png)
<details><summary>Additional WHOIS output</summary>

![WHOIS continued](evidence/screenshots/pm1-whois-2.png)
![WHOIS nameservers](evidence/screenshots/pm1-whois-3.png)
</details>

#### WhatWeb
![WhatWeb technology fingerprint](evidence/screenshots/pm1-whatweb.png)

#### Nslookup
![Nslookup resolution](evidence/screenshots/pm1-nslookup.png)

#### Curl HTTP headers
![Curl response headers](evidence/screenshots/pm1-curl.png)

#### Wafw00f
![WAF detection](evidence/screenshots/pm1-wafw00f.png)

#### DNSRecon
![DNS enumeration](evidence/screenshots/pm1-dnsrecon.png)

---

## PM2 — GHDB and Search-Based OSINT

### Mathematics PDF discovery

Google query used:

```text
filetype:pdf mathematics site:edu
```

Ten educational PDF listings were observed from institutional domains including Carnegie Mellon University, Stanford University, UC Davis, MIT OpenCourseWare and other educational repositories. Files were not redistributed in this repository.

### Mathematics Search Evidence

![Mathematics PDF results 1](evidence/screenshots/pm2-math-1.png)
![Mathematics PDF results 2](evidence/screenshots/pm2-math-2.png)

### Camera exposure methodology

Example assignment-provided dork:

```text
intitle:"webcamXP" inurl:8080
```

The query demonstrated that search engines may index exposed camera-management interfaces. Further third-party camera validation was stopped because public accessibility does not constitute authorization. Live feeds, device addresses and credentials are not included in this repository.

**Recommended safe testing alternative:** instructor-controlled cameras, honeypots or devices covered by written authorization.

### Redacted GHDB Evidence

The screenshot below retains the query and search methodology but redacts the direct third-party device address. A live-feed screenshot is deliberately excluded.

![Redacted GHDB camera search](evidence/screenshots/pm2-ghdb-redacted.png)

---

## PM3 — Footprinting with Maltego

A Domain entity was created for `networkwalks.com`. The following public-data transforms were run:

- `To Email Addresses [Search Engine]`
- `To Emails @domain [Search Engine]`

One unique public organizational address was returned: `info@networkwalks.com`. The second transform did not add another unique address. Search-engine results may be incomplete or outdated and require verification.

### PM3 Evidence

<details><summary>Maltego setup and domain entity</summary>

![Maltego main screen](evidence/screenshots/pm3-setup.png)
![Networkwalks domain entity](evidence/screenshots/pm3-domain.png)
</details>

![Email transform menu](evidence/screenshots/pm3-menu.png)
![Final Maltego result](evidence/screenshots/pm3-result.png)

---

## PM4 — Footprinting with theHarvester

Target: `microsoft.com`, as specified in the assignment.

### Task 1 — Baidu, limit 1000

```bash
theHarvester -d microsoft.com -l 1000 -b baidu
```

The current theHarvester 4.11.1 integration completed without returning IPs, emails, people or hosts. This demonstrates that third-party data sources and parsers can change over time.

![Baidu source result](evidence/screenshots/pm4-baidu.png)

### Task 2 — All sources, limit 50

```bash
theHarvester -d microsoft.com -l 50 -b all
```

The run attempted several public sources. Some required API keys or returned provider-specific errors. Responding sources collectively reported:

- 9 ASNs
- 134 IP addresses
- 3 email addresses
- 9,964 hosts/subdomains
- 1 interesting URL

The full host inventory is intentionally excluded from this public repository.

### PM4 Evidence

<details><summary>Installed version and supported sources</summary>

![theHarvester help](evidence/screenshots/pm4-help-1.png)
![theHarvester supported sources](evidence/screenshots/pm4-help-2.png)
</details>

![All-sources run](evidence/screenshots/pm4-all-start.png)
![Source processing and limitations](evidence/screenshots/pm4-all-sources.png)
![Aggregate results](evidence/screenshots/pm4-all-summary.png)
![Representative final results](evidence/screenshots/pm4-all-results.png)

---

## PM5 — Network Discovery with Nmap CLI

Zenmap is Nmap's graphical interface. I used Nmap CLI to reproduce the required Ping Scan on my own virtual LAN.

### Network details

| Item | Result |
|---|---|
| Interface | `eth0` |
| Kali address | `192.168.122.235/24` |
| Subnet | `192.168.122.0/24` |
| Gateway | `192.168.122.1` |
| Discovery command | `sudo nmap -sn 192.168.122.0/24` |
| Live hosts | 2 |

The `-sn` option performed host discovery only; no port, service-version or vulnerability scan was performed.

### PM5 Evidence

![IP address and route](evidence/screenshots/pm5-network.png)
![Nmap ping scan](evidence/screenshots/pm5-scan.png)
![Kali MAC address](evidence/screenshots/pm5-mac.png)

### Topology

![Nmap host-discovery topology](evidence/PM5-Nmap-Topology.png)

---

## Risk Summary

| Observation | Potential impact | Level |
|---|---|---|
| Public CMS and plugin fingerprints | May help prioritize targeted security research | Medium |
| Public server IP and DNS information | Supports infrastructure mapping | Low |
| HTTP headers and REST API links | Supports application fingerprinting | Low |
| WAF technology identifiable | Reveals part of the defensive architecture | Low |
| Large public subdomain inventory | Can increase asset-management complexity | Medium |
| Internet-indexed camera interfaces | Can create serious privacy and physical-security exposure | High |
| Hosts visible on a local network | Unknown devices may require investigation | Low |

These levels are educational assessments; no finding was exploited or confirmed as a vulnerability.

## Recommendations

1. Keep CMS platforms, plugins and server components updated.
2. Remove unnecessary technology and version disclosures.
3. Review public DNS and certificate-derived assets regularly.
4. Keep the WAF updated, tuned and monitored.
5. Maintain an accurate external asset inventory.
6. Place camera systems behind authentication, segmentation and VPN access.
7. Perform authorized internal host discovery periodically.
8. Investigate unknown network devices.
9. Redact sensitive data before publishing security evidence.
10. Obtain written authorization before every security assessment.

## Key Learning Outcomes

- Reconnaissance tools reveal different parts of an organization's public footprint.
- Tool results change as data providers and APIs evolve.
- Nmap CLI can reproduce Zenmap host-discovery functionality.
- An observation is not automatically a vulnerability.
- Discoverability and accessibility do not replace authorization.
- Ethical scope and responsible evidence handling are essential cybersecurity skills.

## Report

The complete project report is available here:

[Download the Week 2 Project Report](report/Shiv-Kumar-Das-Week2-Project-Report.docx)

## Disclaimer

This repository is for education and defensive-security learning only. It does not authorize testing of any system mentioned here. Raw outputs, live-camera imagery, working third-party device addresses and other unnecessary sensitive evidence have been excluded.
