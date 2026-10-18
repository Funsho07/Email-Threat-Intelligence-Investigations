
# Phishing Email Investigation Report — DHL Delivery Fee Scam

## TargetCorp Threat Intelligence & Email Security Investigation

## 1. Executive Summary

This investigation examines a suspicious email impersonating DHL Express. The message claims that a parcel delivery failed and urges the recipient to reschedule delivery by paying a **$2.99 redelivery administration fee**. It also threatens to return the parcel if it is not claimed within five business days.

The email contains several indicators consistent with a potential phishing attempt:

- The displayed sender uses `noreply@dhl.com`, while the Reply-To address uses `support@dhl-delivery-info.xyz`.
- The message requests payment through a delivery-rescheduling process.
- The email references a ZIP attachment named `ShippingDocs_1928374650.zip`.
- SPF, DKIM, and DMARC authentication checks failed in the supplied evidence.
- VirusTotal identified the domain `dhl-delivery-info.xyz` as suspicious through two security-vendor detections.
- The two public IP addresses in the observed email route have historical AbuseIPDB reports, although both displayed 0% confidence of abuse.

The combined evidence supports classifying the email as **highly suspicious and consistent with a possible delivery-fee phishing attempt**. The available evidence does not establish that the attachment contains malware, that the recipient paid the fee, or that a device or account was compromised.

## 2. Investigation Objectives

- Analyze the email's content, delivery claims, and payment request.
- Examine sender and Reply-To inconsistencies.
- Review the email headers and sending infrastructure.
- Investigate the public IP addresses using AbuseIPDB.
- Review the domain using VirusTotal.
- Evaluate email authentication and routing using MXToolbox.
- Document indicators of concern and recommend defensive actions.

## 3. Tools and Evidence Sources

- **Email sample and headers:** Message content, sender details, and routing information.
- **AbuseIPDB:** Public IP reputation and historical reports.
- **VirusTotal:** Domain reputation and security-vendor detections.
- **MXToolbox:** Email authentication and header analysis.
- **Manual analysis:** Social-engineering indicators, payment requests, and domain inconsistencies.

## 4. Email Content Analysis

### 4.1 Email Summary

| Attribute | Observed details |
|---|---|
| Display name | DHL Express - Delivery Notification |
| From address | `noreply@dhl.com` |
| Reply-To address | `support@dhl-delivery-info.xyz` |
| Recipient | `recipient@targetcorp.com` |
| Subject | DHL: Your Parcel Could Not Be Delivered - Tracking #1928374650 |
| Tracking number | `1928374650` |
| Claimed parcel sender | Amazon.com |
| Estimated parcel weight | 2.3 kg |
| Delivery attempt | 1 of 3 |
| Claimed delivery attempt time | 11:34 AM |
| Stated hold deadline | April 24, 2026 |
| Requested redelivery fee | $2.99 |
| Referenced attachment | `ShippingDocs_1928374650.zip` |

### Supporting Evidence: Email Sample

![DHL delivery notification email](../screenshots/email-03/suspicious%20email.png)

### 4.2 Urgency and Social Engineering

The email uses a prominent red delivery-failure banner and demands action within five days. It claims that the parcel could be returned to the sender if the recipient does not complete the rescheduling process.

The message also requests a $2.99 administration fee and presents a prominent **Reschedule My Delivery** button.

These elements may pressure the recipient into clicking a link and supplying payment information. The relatively small fee could make the request appear routine and reduce suspicion.

The email opens with the generic greeting "Dear DHL Customer" rather than identifying the recipient by name. This is another contextual warning sign, although legitimate automated notifications can also use generic greetings.

**Evidence:**

![Urgent delivery warning and deadline](../screenshots/email-03/urgency%20prompts.png)

![Generic greeting, parcel details, fee request, and rescheduling button](../screenshots/email-03/CC%20harvesting.png)

### 4.3 Sender and Reply-To Analysis

The message displays a DHL sender address but directs replies to a different domain.

| Field | Observed value |
|---|---|
| From | `noreply@dhl.com` |
| Reply-To | `support@dhl-delivery-info.xyz` |
| Header From domain | `dhl.com` |
| Envelope sender | `tracking@dhl-delivery-info.xyz` |

The mismatch between `dhl.com` and `dhl-delivery-info.xyz` is significant. The alternate domain is not the same as the displayed DHL domain and is associated with the delivery-fee request.

The header evidence also shows that the envelope sender uses the alternate domain. This discrepancy, combined with the failed authentication results and payment request, increases the likelihood that the email is fraudulent.

A domain mismatch alone is not conclusive proof of phishing, but the combined evidence warrants treating the message as suspicious.

**Evidence:**

![Sender and Reply-To comparison](../screenshots/email-03/sender%20and%20reply%20to.png)

## 5. Email Header and Authentication Analysis

### 5.1 Observed Sending Infrastructure

The supplied headers and MXToolbox results identify two public IP addresses in the observed email route.

| Observed component | Value |
|---|---|
| Initial internal hop | `172.16.88.9` |
| Sending server | `smtp.dhl-delivery-info.xyz` |
| Public IP at sending hop | `176.111.174.33` |
| Subsequent mail server | `mail.dhl-delivery-info.xyz` |
| Public IP at subsequent hop | `176.111.174.55` |
| Receiving server | `mx.google.com` |

The address `172.16.88.9` is a private IP address and is not a publicly routable Internet address. It should not be treated as a public attacker IP.

The two public IP addresses are relevant because they appear in the supplied routing evidence. Their presence does not independently identify the operator of the infrastructure.

**Evidence:**

![Different IP addresses observed in the email route](../screenshots/email-03/different%20IPs.png)

### 5.2 Authentication Results

The supplied authentication results show:

- **SPF: Fail** — the evaluated SPF check reports that `176.111.174.55` is not authorized by the evaluated `dhl.com` SPF policy.
- **DKIM: Fail** — the authentication result reports that no key was found for the signature in DNS.
- **DMARC: Fail** — the message failed the evaluated DMARC check.
- **Domain alignment failure:** The displayed From domain, `dhl.com`, does not align with the envelope sender domain, `dhl-delivery-info.xyz`.

The authentication evidence undermines confidence that the message was properly authenticated as originating from the displayed DHL domain.

Authentication failures can also result from configuration problems or message alteration. In this investigation, however, they appear alongside the suspicious Reply-To domain and the request for a redelivery fee.

**Evidence:**

![Email authentication results](../screenshots/email-03/Authentication%20result.png)

### 5.3 MXToolbox Analysis

The MXToolbox results show failures for DMARC compliance, SPF alignment, SPF authentication, DKIM alignment, and DKIM authentication.

The routing details also identify the two public IP addresses discussed in Section 6.

These results support the concern that the message was not properly authenticated as originating from the displayed sender. They should be considered together with the message content and domain mismatch rather than treated as proof of malicious activity on their own.

**Evidence:**

![MXToolbox email authentication results](../screenshots/email-03/MXtoolbox%201.png)

![MXToolbox message routing and delivery details](../screenshots/email-03/Mxtoolbox%202.png)

![MXToolbox authentication diagnostics](../screenshots/email-03/Mxtoolbox%203.png)

## 6. Threat Intelligence and Domain Reputation

### 6.1 IP Address: `176.111.174.55`

AbuseIPDB returned the following information at the time of analysis:

| Attribute | Result |
|---|---|
| Abuse reports | 4 |
| Confidence of abuse | 0% |
| ISP | NUBES LLC |
| Usage type | Data Center/Web Hosting/Transit |
| ASN | AS212136 |
| Domain name | `nubes.ru` |
| Country | Russian Federation |
| City | Moscow |
| Most recent report | Approximately two years before the lookup |

The historical reports include RDP brute-force activity, automated login attempts involving PostgreSQL, MSSQL brute-force activity, and port scanning.

The report count and historical categories provide context about previous reports associated with the IP address. However, the displayed confidence score is 0%, and the most recent report is old. These results do not establish that the address was malicious during this email's delivery.

**Evidence:**

![AbuseIPDB summary for 176.111.174.55](../screenshots/email-03/AbuseIPDB%20176.111.174.55.png)

![Historical AbuseIPDB reports for 176.111.174.55](../screenshots/email-03/AbuseIPDB2%20176.111.174.55.png)

### 6.2 IP Address: `176.111.174.33`

AbuseIPDB returned the following information:

| Attribute | Result |
|---|---|
| Abuse reports | 1 |
| Confidence of abuse | 0% |
| ISP | NUBES LLC |
| Usage type | Data Center/Web Hosting/Transit |
| ASN | AS212136 |
| Domain name | `nubes.ru` |
| Country | Russian Federation |
| City | Moscow |
| Most recent report | Approximately two years before the lookup |

The supplied historical report describes RDP brute-force activity.

The IP address appears in the email headers as part of the observed sending route. Its presence is relevant to the investigation, but the single historical report and 0% confidence score do not prove that it was malicious during the incident or identify the person responsible for sending the message.

**Evidence:**

![AbuseIPDB summary for 176.111.174.33](../screenshots/email-03/AbuseIPDB%20176.111.174.33.png)

![Historical AbuseIPDB report for 176.111.174.33](../screenshots/email-03/AbuseIPDB2%20176.111.174.33.png)

### 6.3 Domain: `dhl-delivery-info.xyz`

The supplied VirusTotal screenshot records an analysis of `http://dhl-delivery-info.xyz/` on October 17, 2026, at 16:17:48 UTC.

| Attribute | Observed result |
|---|---|
| Domain | `dhl-delivery-info.xyz` |
| Community score | 0/93 |
| Forcepoint ThreatSeeker | Suspicious |
| LevelBlue | Suspicious |

Two security-vendor detections in the supplied screenshot classify the domain as suspicious.

The community score of 0/93 should not be interpreted as proof that the domain is safe. Security-vendor results can differ, and a domain may not yet have widespread detection coverage.

The VirusTotal findings strengthen the concern about the alternate domain, especially when considered alongside the sender mismatch, delivery-fee request, and authentication failures. They do not independently prove that the domain hosted malware or that a recipient was compromised.

**Evidence:**

![VirusTotal analysis of dhl-delivery-info.xyz](../screenshots/email-03/Virus%20total.png)

## 7. Consolidated Indicators of Concern

| Indicator | Observation | Significance |
|---|---|---|
| Displayed sender | `noreply@dhl.com` | Claims to represent DHL |
| Reply-To | `support@dhl-delivery-info.xyz` | Differs from the displayed sender domain |
| Envelope sender | `tracking@dhl-delivery-info.xyz` | Uses the alternate domain |
| Domain under investigation | `dhl-delivery-info.xyz` | Two vendors marked it suspicious in the supplied VirusTotal evidence |
| Payment request | $2.99 redelivery administration fee | Potential payment-information harvesting |
| Social engineering | Urgency, deadline, and threat of parcel return | Encourages quick action |
| ZIP attachment | `ShippingDocs_1928374650.zip` | Unverified attachment requiring caution |
| Public IP | `176.111.174.33` | One historical AbuseIPDB report; 0% confidence |
| Public IP | `176.111.174.55` | Four historical AbuseIPDB reports; 0% confidence |
| SPF | Fail | Evaluated sending IP not authorized by the evaluated policy |
| DKIM | Fail | No key found for the signature in DNS |
| DMARC | Fail | Authentication evaluation failed |
| Domain alignment | Fail | Displayed From and envelope sender domains do not align |

## 8. Key Findings

1. **Potential delivery-fee phishing:** The email requests a small payment to reschedule a parcel delivery.
2. **Sender impersonation indicators:** The displayed DHL sender differs from the Reply-To and envelope sender domains.
3. **Suspicious domain reputation:** Two security vendors marked `dhl-delivery-info.xyz` as suspicious in the supplied VirusTotal evidence.
4. **Authentication failures:** SPF, DKIM, and DMARC checks failed in the analyzed message.
5. **Relevant sending infrastructure:** Two public IP addresses appear in the email route and have historical AbuseIPDB reports.
6. **Limited IP attribution:** Both IP addresses show 0% AbuseIPDB confidence scores in the supplied screenshots. Their historical reports do not prove current malicious activity.
7. **Unverified attachment:** The email references a ZIP attachment, but the available evidence does not establish its contents or whether it is malicious.
8. **No confirmed compromise:** The investigation does not establish that the recipient paid the fee, opened the attachment, submitted information, or experienced a successful compromise.

## 9. Recommended Response and Defensive Actions

- Do not click the rescheduling button or reply to the suspicious address.
- Do not open the ZIP attachment unless it has been independently verified and safely analyzed.
- Verify delivery status through DHL's official website or customer-service channel, reached independently rather than through the email.
- Do not provide payment-card details, credentials, or personal information through the supplied link.
- Preserve the original email, full headers, attachment, screenshots, and timestamps.
- If the link was clicked or information was submitted, notify the appropriate security team and follow incident-response procedures.
- If payment information was submitted, contact the financial institution promptly.
- Review mail gateway logs for similar messages and search for related sender addresses, domains, and message subjects.
- Consider investigating the suspicious domain and associated infrastructure using approved threat-intelligence processes.
- Review email authentication controls and ensure SPF, DKIM, and DMARC policies are correctly configured for legitimate organizational domains.

## 10. Conclusion

The analyzed DHL-themed email contains multiple indicators consistent with a possible parcel-delivery phishing attempt. The redelivery fee, urgent deadline, sender/Reply-To mismatch, failed email authentication, and suspicious VirusTotal detections collectively justify treating the message as highly suspicious.

The AbuseIPDB results provide historical context but do not independently establish that either IP address was malicious during this incident. The ZIP attachment has not been confirmed as malicious, and no financial loss or successful compromise has been established.

The recommended disposition is to **treat the email as suspicious, avoid interacting with its links or attachment, and verify the claimed delivery through an independent official channel**.
