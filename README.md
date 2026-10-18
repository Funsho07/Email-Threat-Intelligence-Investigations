
# Email Threat Intelligence & Phishing Investigations

A cybersecurity portfolio showcasing email threat analysis, open-source intelligence (OSINT), indicator-of-compromise (IOC) assessment, and email security investigation techniques.

## Project Overview

This portfolio presents four structured investigations into suspicious emails involving Microsoft account impersonation, invoice payment redirection, DHL delivery phishing, and banking-security impersonation.

Each investigation examines email content, sender identity, message headers, suspicious domains and IP addresses, threat-intelligence results, and email-authentication indicators. Findings are correlated to assess risk, identify potential phishing techniques, and recommend appropriate defensive actions.

## Technical Skills Demonstrated

- **Email Security Analysis:** Sender, Reply-To, Return-Path, and message-header investigation.
- **Threat Intelligence & OSINT:** IP reputation research, domain analysis, and interpretation of external intelligence.
- **Email Authentication:** SPF, DKIM, DMARC, and alignment analysis.
- **IOC Assessment:** Identification and evaluation of suspicious IP addresses, domains, and URLs.
- **Security Reporting:** Evidence documentation, analytical findings, risk assessment, and mitigation recommendations.
- **GitHub:** Structured technical documentation and evidence management.

## Investigation Reports

### Email 1 — Microsoft Account Impersonation

Investigation of a Microsoft-themed email claiming that an account had been suspended and demanding verification within 24 hours.

**Focus areas:** Sender and Reply-To discrepancies, social engineering, AbuseIPDB reputation, and email-authentication analysis.

[Read the Email 1 investigation](reports/email-01.md)

### Email 2 — Invoice Fraud and Payment Redirection

Investigation of a suspicious invoice email impersonating Delta Supplies Corp and requesting payment using updated banking details.

**Focus areas:** Invoice fraud indicators, sender and Reply-To analysis, IP reputation, email routing, and authentication failures.

[Read the Email 2 investigation](reports/email-02.md)

### Email 3 — DHL Delivery Notification Phishing

Investigation of a DHL-themed delivery notification claiming a failed delivery and requesting a redelivery fee.

**Focus areas:** Delivery-themed social engineering, suspicious domains, IP reputation, VirusTotal findings, and email authentication.

[Read the Email 3 investigation](reports/email-03.md)

### Email 4 — Bank of America Impersonation

Investigation of a banking-security email claiming unauthorized account access and demanding identity verification and a password reset within 12 hours.

**Focus areas:** Brand impersonation, urgency, sender and Reply-To discrepancies, IP reputation, VirusTotal findings, and SPF/DKIM/DMARC analysis.

[Read the Email 4 investigation](reports/email-04.md)

## Tools Used

- **AbuseIPDB** — IP reputation and abuse-report research.
- **VirusTotal** — Domain and URL threat-intelligence analysis.
- **MXToolbox** — Email-header and authentication analysis.
- **Email header analysis** — Sender identity, Reply-To, Return-Path, routing, and authentication results.
- **GitHub and Markdown** — Technical documentation and evidence organisation.

## Investigation Methodology

Each investigation follows a structured workflow:

1. **Email examination:** Review the subject, message content, sender information, and requested action.
2. **Indicator identification:** Identify urgency, impersonation, suspicious domains, and payment or credential requests.
3. **Header analysis:** Compare sender-related fields and examine routing and authentication results.
4. **Threat intelligence research:** Investigate relevant IP addresses, domains, and URLs using available intelligence sources.
5. **Evidence correlation:** Assess multiple indicators together to develop a reasoned risk assessment.
6. **Reporting and recommendations:** Document findings, investigative limitations, and appropriate defensive actions.

## Repository Structure

```text
Harmony-Health-Threat-Intelligence-Investigation/
├── README.md
├── LICENSE
├── reports/
│   ├── email-01.md
│   ├── email-02.md
│   ├── email-03.md
│   └── email-04.md
└── screenshots/
    ├── email-01/
    ├── email-02/
    ├── email-03/
    └── email-04/
```

## Project Objectives

- Develop a repeatable approach to investigating suspicious emails.
- Strengthen practical skills in email security analysis and threat intelligence.
- Correlate technical indicators with social-engineering techniques.
- Produce clear, evidence-based investigation reports.
- Communicate findings and recommend actionable defensive measures.


