
# Phishing Email Investigation Report — Bank of America Impersonation

## TargetCorp Threat Intelligence & Email Security Investigation

## 1. Executive Summary

This investigation examines a suspicious email impersonating Bank of America. The message claims that unauthorized access to an online banking account was detected and that the account has been temporarily restricted pending identity verification.

The email creates urgency by demanding action within 12 hours and encouraging the recipient to verify their identity and reset their password. It also claims that a successful unauthorized login occurred from IP address `185.220.101.47`, allegedly located in Bucharest, Romania.

The investigation identified several indicators of concern:

- The displayed sender address uses `security.alert@bankofamerica.com`, while the Reply-To address uses `security-noreply@secure-boa-alerts.cc`.
- The email's public sending infrastructure uses the domain `secure-boa-alerts.cc`.
- SPF, DKIM, and DMARC authentication checks failed in the supplied evidence.
- The DKIM diagnostic reports a 512-bit RSA key, which is too weak.
- VirusTotal showed no security vendors flagging `secure-boa-alerts.cc` as malicious in the summary, but alphaMountain.ai classified it as suspicious.
- AbuseIPDB reported 6,849 reports and 100% confidence of abuse for `185.220.101.47`, which the email identifies as the alleged suspicious-login IP.
- The separate public mail-server IP `91.108.56.122` had two AbuseIPDB reports and a 9% confidence score.

The combined findings support classifying the message as **highly suspicious and consistent with a possible banking-themed credential-phishing attempt**. However, the investigation does not establish that the recipient clicked the link, disclosed credentials, or experienced a successful account compromise.

## 2. Investigation Objectives

- Analyze the email content and claimed security incident.
- Examine the sender and Reply-To addresses.
- Review email headers and the sending infrastructure.
- Investigate the relevant public IP addresses using AbuseIPDB.
- Review the sender domain using VirusTotal.
- Evaluate SPF, DKIM, DMARC, and alignment results using MXToolbox.
- Document indicators of concern and recommend defensive actions.

## 3. Tools and Evidence Sources

- **Email sample and headers:** Message content, sender details, and routing information.
- **AbuseIPDB:** Public IP reputation and historical abuse reports.
- **VirusTotal:** Domain reputation and security-vendor detections.
- **MXToolbox:** Email authentication, header, and routing analysis.
- **Manual analysis:** Social-engineering indicators and discrepancies between the displayed sender and actual email infrastructure.

## 4. Email Content Analysis

### 4.1 Email Summary

| Attribute | Observed details |
|---|---|
| Display name | Bank of America Security Center |
| From address | `security.alert@bankofamerica.com` |
| Reply-To address | `security-noreply@secure-boa-alerts.cc` |
| Recipient | `user@targetcorp.com` |
| Subject | SECURITY ALERT: Unusual Activity Detected - Immediate Password Reset Required |
| Case reference | `BOA-SEC-2026-119847` |
| Claimed detection time | April 18, 2026 — 13:47 UTC |
| Claimed incident | One successful unauthorized login |
| IP cited in the email body | `185.220.101.47` |
| Claimed location | Bucharest, Romania |
| Claimed account status | Temporarily restricted |
| Required action | Verify identity and reset password within 12 hours |

### Supporting Evidence: Email Sample

![Bank of America security alert email](../screenshots/email-04/BOA%20suspicious%20email.png)

### 4.2 Urgency and Social Engineering

The message uses a high-priority security alert, a case reference, and a warning that the account has been temporarily restricted. It states that action is required within 12 hours and threatens continued account restriction and automatic fraud-case opening if verification is not completed.

The prominent **Verify Identity & Reset Password Now** button encourages the recipient to take immediate action.

These techniques may pressure a recipient into clicking a link and entering banking credentials on a fraudulent page. The generic greeting, "Dear Valued Customer," is another contextual warning sign, although legitimate automated messages can also use generic greetings.

The email's claims should not be treated as verified banking activity without independent confirmation.

**Evidence:**

![Urgent security alert and account restriction claim](../screenshots/email-04/BOA%20suspicious%20email.png)

## 5. Email Header and Authentication Analysis

### 5.1 Sender and Reply-To Analysis

The email displays a Bank of America sender address but directs replies to a different domain.

| Header field | Observed value |
|---|---|
| Display name | Bank of America Security Center |
| From | `security.alert@bankofamerica.com` |
| Reply-To | `security-noreply@secure-boa-alerts.cc` |
| Recipient | `user@targetcorp.com` |
| Envelope sender | `noreply@secure-boa-alerts.cc` |

The difference between `bankofamerica.com` and `secure-boa-alerts.cc` is significant. The alternate domain is not the same as the displayed sender domain and appears in both the Reply-To and envelope-sender information.

This discrepancy is consistent with a possible impersonation attempt. It becomes more concerning when considered alongside the failed authentication checks and the request to verify banking credentials.

A sender-address discrepancy alone does not prove malicious intent, but the combined evidence warrants treating the message as suspicious.

**Evidence:**

![From and Reply-To header comparison](../screenshots/email-04/Sender%20and%20reply%20path.png)

### 5.2 Observed Email Routing

The supplied headers show the following mail-server information:

| Observed component | Value |
|---|---|
| First internal hop | `10.20.30.45` |
| Sending server | `smtp.secure-boa-alerts.cc` |
| Public IP at sending hop | `91.108.56.99` |
| Subsequent mail server | `mailer.secure-boa-alerts.cc` |
| Public IP at subsequent hop | `91.108.56.122` |
| Receiving server | `mx.google.com` |
| X-Originating-IP | `91.108.56.122` |
| X-Mailer | SwiftMailer v6.3.0 |

The address `10.20.30.45` is a private IP address and is not publicly routable on the Internet. It should not be treated as a separate public threat-intelligence indicator.

The public IP addresses appear in the supplied email headers and routing evidence. The evidence does not independently establish the identity of the person operating the infrastructure.

**Evidence:**

![Public IP addresses and email routing details](../screenshots/email-04/IP%20addresses%20for%20BOA.png)

### 5.3 Email Authentication Results

The supplied authentication results report:

- **SPF: Fail** — the evaluated sending IP `91.108.56.122` is not authorized by the evaluated `bankofamerica.com` SPF policy.
- **DKIM: Fail** — signature verification failed, and the diagnostic reports that the RSA key size is 512 bits and too weak.
- **DMARC: Fail** — the evaluated message failed DMARC and the displayed policy result indicates rejection.
- **Domain alignment failure** — the displayed From domain, `bankofamerica.com`, does not align with the envelope sender domain, `secure-boa-alerts.cc`.

These results undermine confidence that the message was properly authenticated as originating from the displayed Bank of America domain.

Authentication failures can sometimes result from configuration problems or message alteration. In this case, they appear alongside the alternate Reply-To domain and the demand for urgent identity verification, making the overall message substantially more suspicious.

**Evidence:**

![Email authentication results for the Bank of America-themed message](../screenshots/email-04/Authentication%20result%20BOA.png)

### 5.4 MXToolbox Analysis

The supplied MXToolbox results identify failures involving DMARC compliance, SPF alignment, SPF authentication, DKIM alignment, and DKIM authentication.

Additional diagnostics report an invalid DKIM signature and an insufficiently strong 512-bit RSA key. The supplied analysis also reports no SPF record found in the evaluated diagnostic.

These findings support the concern that the message was not properly authenticated as originating from the displayed sender. They should be interpreted together with the email's content and domain mismatch rather than treated as proof of malicious activity on their own.

**Evidence:**

![MXToolbox authentication and delivery analysis](../screenshots/email-04/Mxtoolbox%20BOA.png)

![MXToolbox SPF and DKIM diagnostics](../screenshots/email-04/Mxtoolbox%20BOA2.png)

![MXToolbox DKIM signature diagnostics](../screenshots/email-04/Mxtoolbox%20BOA3.png)

![MXToolbox additional authentication details](../screenshots/email-04/Mxtoolbox%20BOA4.png)

## 6. Threat Intelligence and IP Reputation

### 6.1 IP Address: `185.220.101.47`

The email body identifies this IP address as the source of the alleged unauthorized banking login. AbuseIPDB returned the following information at the time of analysis:

| Attribute | Result |
|---|---|
| Abuse reports | 6,849 |
| Distinct reporting sources | 649 |
| Confidence of abuse | 100% |
| Classification | Tor exit node |
| Hostname | `tor-exit-47.for-privacy.net` |
| Domain | `for-privacy.net` |
| ASN | AS60729 |
| Location reported | Berlin, Germany |
| First reported | September 22, 2021 |
| Most recent report shown | October 17, 2026 |

The supplied recent-report evidence includes reports involving exploit activity detected by CrowdSec, repeated WordPress login/XML-RPC attempts, port scanning, web-application attacks, and unauthorized VPN login attempts.

The high report count and confidence score indicate a strong negative reputation in AbuseIPDB at the time of lookup. The screenshot also identifies the address as a Tor exit node.

**Important limitation:** This IP is cited in the email body as the alleged source of the banking login. The available evidence does not independently verify that a login occurred, that the bank attributed the event to this address, or that the address was responsible for sending the email. The AbuseIPDB reputation is relevant context, not proof of the claimed banking incident.

**Evidence:**

![AbuseIPDB reputation summary for 185.220.101.47](../screenshots/email-04/AbuseIPDB%20185.220.101.47.png)

![Recent AbuseIPDB reports for 185.220.101.47](../screenshots/email-04/AbuseIPDB..2%20185.220.101.47.png)

### 6.2 IP Address: `91.108.56.122`

AbuseIPDB returned the following information for the public IP observed in the email headers:

| Attribute | Result |
|---|---|
| Abuse reports | 2 |
| Distinct reporting sources | 2 |
| Confidence of abuse | 9% |
| ISP | Telegram Messenger Network |
| Usage type | Data Center/Web Hosting/Transit |
| ASN | AS62014 |
| Domain name | `telegram.org` |
| Country | Singapore |
| Most recent report shown | Approximately two weeks before the lookup |

The supplied historical reports refer to port scanning and traffic blocked by a security appliance.

The report count and 9% confidence score provide limited evidence of previous reported activity. They do not independently prove that this IP was malicious during the delivery of the investigated email.

The IP's appearance in the email headers makes it relevant to the investigation, but reputation information alone cannot establish the identity or intent of the infrastructure operator.

**Evidence:**

![AbuseIPDB reputation summary for 91.108.56.122](../screenshots/email-04/AbuseIPDB%2091.108.56.122.png)

![Historical AbuseIPDB reports for 91.108.56.122](../screenshots/email-04/AbuseIPDB.2%2091.108.56.122.png)

### 6.3 Domain: `secure-boa-alerts.cc`

The supplied VirusTotal screenshot shows the following results:

| Attribute | Observed result |
|---|---|
| Domain | `secure-boa-alerts.cc` |
| Community score | 0/92 |
| Security vendors flagging the domain as malicious in the summary | None |
| alphaMountain.ai | Suspicious |

The results are mixed. The screenshot does not show a broad malicious verdict from security vendors, but alphaMountain.ai classifies the domain as suspicious.

A community score of 0/92 does not establish that a domain is safe. Detection coverage can be incomplete, and vendor classifications may differ.

The suspicious classification supports further investigation when combined with the email's sender mismatch, authentication failures, and credential-verification request. It does not independently prove that the domain hosted malware or that a compromise occurred.

**Evidence:**

![VirusTotal analysis of secure-boa-alerts.cc](../screenshots/email-04/virustotal%20boa.png)

## 7. Consolidated Indicators of Concern

| Indicator | Observation | Significance |
|---|---|---|
| Displayed sender | `security.alert@bankofamerica.com` | Claims to represent Bank of America |
| Reply-To | `security-noreply@secure-boa-alerts.cc` | Differs from the displayed sender domain |
| Envelope sender | `noreply@secure-boa-alerts.cc` | Uses the alternate domain |
| Email infrastructure domain | `secure-boa-alerts.cc` | Appears in the email's routing information |
| Domain reputation | alphaMountain.ai marked the domain suspicious | Supports further investigation |
| Body IP | `185.220.101.47` | 6,849 AbuseIPDB reports and 100% confidence; cited as the alleged login source |
| Header IP | `91.108.56.122` | Two AbuseIPDB reports and 9% confidence |
| Additional header IP | `91.108.56.99` | Appears in the supplied routing evidence; no separate reputation result documented here |
| SPF | Fail | Evaluated sending IP not authorized by the evaluated policy |
| DKIM | Fail | Signature verification failed; 512-bit RSA key reported as too weak |
| DMARC | Fail | Authentication evaluation failed |
| Social engineering | Urgent security warning and 12-hour deadline | Encourages immediate action |
| Requested action | Verify identity and reset password | Potential credential-harvesting objective |

## 8. Key Findings

1. **Potential banking impersonation:** The email presents itself as a Bank of America security alert but uses an alternate Reply-To and envelope-sender domain.
2. **Credential-phishing indicators:** The message demands urgent identity verification and password resetting.
3. **Authentication failures:** SPF, DKIM, and DMARC checks failed in the supplied evidence.
4. **Weak DKIM key:** The diagnostic reports a 512-bit RSA key and signature verification failure.
5. **Suspicious domain result:** VirusTotal's supplied screenshot identifies alphaMountain.ai as classifying `secure-boa-alerts.cc` as suspicious.
6. **Strong reputation signal for the body IP:** AbuseIPDB reports 6,849 reports and 100% confidence for `185.220.101.47`, but this does not prove the alleged banking login occurred.
7. **Limited reputation signal for the header IP:** `91.108.56.122` has two historical reports and a 9% confidence score; this alone is not proof of malicious activity in the incident.
8. **No confirmed compromise:** The available evidence does not establish that the recipient clicked the link, supplied credentials, or experienced a successful compromise.

## 9. Recommended Response and Defensive Actions

- Do not click the identity-verification button or reply to the suspicious address.
- Verify any claimed banking security incident through Bank of America's official website or telephone number obtained independently.
- Do not provide passwords, PINs, one-time passcodes, or other sensitive information through the email's links.
- Preserve the original email, full headers, attachments, screenshots, and relevant timestamps.
- Search mail gateway and security logs for related messages, sender addresses, domains, and indicators.
- Investigate `secure-boa-alerts.cc` and associated infrastructure using approved threat-intelligence procedures.
- Review the organization's email authentication configuration and investigate why the evaluated SPF, DKIM, and DMARC checks failed.
- If credentials were entered, contact the bank promptly, change the affected password through the official channel, and review account activity.
- If the recipient clicked a link or opened an attachment, notify the appropriate security team and follow incident-response procedures.
- Monitor for similar messages that impersonate financial institutions or request urgent credential verification.

## 10. Conclusion

The investigated email is highly suspicious and consistent with a possible Bank of America impersonation and credential-phishing attempt. The sender/Reply-To discrepancy, alternate sending domain, failed email authentication, urgent verification request, and suspicious VirusTotal result collectively support this assessment.

The high AbuseIPDB reputation of `185.220.101.47` is relevant because the email cites it as the alleged source of an unauthorized login. However, the evidence does not independently verify the claimed login or link that IP to the email's sender. The reputation of `91.108.56.122` is comparatively weak and should not be overstated.

The recommended disposition is to **treat the email as suspicious, avoid interacting with its links, and verify the claimed security incident through an independent official banking channel**. No successful credential theft or account compromise has been established by the available evidence.
