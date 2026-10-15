# Harmony-Health-Threat-Intelligence-Investigation
Threat Intelligence 
# Harmony Health Breach Investigation — Threat Intelligence & OSINT

## Project Overview
A cybersecurity case study focused on investigating a suspected breach at Harmony Health, a healthcare organization with Ubuntu-based servers and Windows endpoints. The scenario explores how SOC analysts use threat intelligence, phishing analysis, SIEM alerts, and network logs to investigate potential unauthorized access and data exfiltration.

## Objectives
- Investigate the suspected cybersecurity breach and assess its potential scope and impact.
- Analyse phishing emails to identify indicators of compromise (IOCs).
- Use OSINT to investigate suspicious IP addresses, domains, URLs, and email artifacts.
- Correlate endpoint and network logs to identify suspicious activity and potential attack patterns.

## Goals
- Identify phishing indicators, spoofed domains, suspicious links, and social-engineering techniques.
- Extract and document IOCs for further investigation.
- Classify indicators as malicious, suspicious, or benign based on available evidence.
- Investigate possible brute-force attempts, credential theft, lateral movement, and data exfiltration.
- Develop an incident timeline and recommend containment, remediation, and security improvements.

## Tools Used
- **Wazuh** — SIEM alert monitoring and log investigation
- **Suricata** — Network threat detection
- **VirusTotal** — File, URL, and domain reputation analysis
- **AbuseIPDB** — IP reputation investigation
- **WHOIS** — Domain registration research
- **URLScan.io** — URL and website investigation
- **Shodan** — Internet-facing infrastructure intelligence
- **MXToolbox** — Email, DNS, and domain diagnostics
- **Ubuntu and Kali Linux** — Security investigation lab
- **pfSense** — Firewall configuration and network segmentation

## Expected Outcomes
- Identify and document relevant phishing indicators and IOCs.
- Enrich suspicious indicators using OSINT and threat intelligence.
- Correlate SIEM alerts with endpoint and network evidence.
- Assess potential unauthorized access, lateral movement, and data exfiltration.
- Produce an evidence-based incident timeline and actionable security recommendations.

## Key Skills
- Threat Intelligence
- Open-Source Intelligence (OSINT)
- Phishing Analysis
- IOC Extraction and Enrichment
- SIEM Investigation
- Network Security Monitoring
- Incident Timeline Development
- Incident Response

## Investigation Workflow
1. Analyse the suspected phishing email.
2. Extract suspicious senders, domains, URLs, and other IOCs.
3. Investigate indicators using OSINT and threat intelligence tools.
4. Review email headers, authentication results, and sender IP information.
5. Correlate findings with Wazuh alerts, Ubuntu and Windows logs, and network traffic.
6. Assess the incident and recommend containment and remediation measures.
