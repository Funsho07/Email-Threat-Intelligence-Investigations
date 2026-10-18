
# Phishing Email Investigation Report — Microsoft Account Impersonation

## TargetCorp Threat Intelligence & Email Security Investigation

## 1. Executive Summary

This investigation examines a suspicious email impersonating Microsoft's account security team. The message claims that the recipient's Microsoft account has been temporarily suspended because of suspicious sign-in activity and demands identity verification within 24 hours.

The investigation identified several warning signs, including a mismatch between the displayed sender and Reply-To address, a separate Return-Path domain, email-authentication failures, and an IP address with a substantial negative reputation in AbuseIPDB.

The available evidence supports classifying the email as **highly suspicious and consistent with a possible credential-phishing attempt**. However, the investigation does not establish that the recipient clicked a link, submitted credentials, or experienced a successful account compromise.

## 2. Investigation Objectives

- Examine the email content and the account-suspension claim.
- Identify social-engineering techniques used to pressure the recipient.
- Analyse the sender, Reply-To, and Return-Path fields.
- Review the available email-header and authentication evidence.
- Investigate the observed IP address using AbuseIPDB.
- Review email-authentication results using MXToolbox.
- Document indicators of concern and recommend defensive actions.

## 3. Tools and Evidence Sources

- **Email sample and headers:** Message content, sender information, and routing details.
- **AbuseIPDB:** IP reputation and historical abuse reports.
- **MXToolbox:** Email authentication and header analysis.
- **Manual analysis:** Social-engineering indicators and inconsistencies in the message.

## 4. Email Content Analysis

### 4.1 Email Summary

| Attribute | Observed details |
|---|---|
| Displayed sender | Microsoft Account Team |
| From address | `account-security@microsoft.com` |
| Reply-To address | `ms-support@account-ms365-verify.top` |
| Return-Path | `noreply@mailer-srv-dispatch.xyz` |
| Recipient | `analyst@targetcorp.com` |
| Subject | `[Action Required] Your Microsoft Account Has Been Suspended - Verify Now` |
| Claimed issue | Suspicious sign-in attempts and temporary account suspension |
| Stated deadline | Within 24 hours |
| IP shown in email | `91.108.56.122` |
| Claimed sign-in location | Kyiv, Ukraine |

### Supporting Evidence: Phishing Email

![Microsoft account suspension phishing email](../screenshots/email-01/Phish%20email.png)

### 4.2 Urgency and Social Engineering

The email warns that the recipient's account has been temporarily suspended and demands verification within 24 hours. It also threatens possible loss of access to email and OneDrive, subscription cancellation, and permanent account deletion.

These claims create fear and urgency, potentially encouraging the recipient to follow the supplied verification link without independently checking the message.

The generic greeting, dramatic warning, and demand for immediate action are consistent with common social-engineering techniques.

**Assessment:** The message contains several social-engineering indicators consistent with a possible credential-phishing attempt.

## 5. Email Header and Authentication Analysis

### 5.1 Sender, Reply-To, and Return-Path Analysis

The supplied email evidence identifies three different domains across the sender-related fields.

| Header field | Observed value |
|---|---|
| From | `account-security@microsoft.com` |
| Reply-To | `ms-support@account-ms365-verify.top` |
| Return-Path | `noreply@mailer-srv-dispatch.xyz` |

The displayed sender claims to represent Microsoft, but the Reply-To and Return-Path use different domains.

This discrepancy is significant because replies or other interactions may be directed to infrastructure unrelated to the displayed sender. Although such a mismatch is not conclusive proof of phishing by itself, it becomes more concerning when combined with the urgent account-verification request and authentication failures.

**Evidence:**

![Email header showing sender and Reply-To discrepancy](../screenshots/email-01/email%20header%20analysis.png)

### 5.2 Email Authentication Results

The supplied MXToolbox results report:

- DMARC compliance failure.
- SPF alignment failure.
- SPF authentication failure.
- DKIM authentication failure.
- DKIM alignment reported as passing in the displayed results.

The DKIM alignment result is an important distinction: not every authentication check shown in the evidence failed. The overall authentication results nevertheless raise concerns about whether the message was properly authenticated as originating from the claimed Microsoft domain.

Authentication failures can sometimes result from misconfiguration or message alteration. They should therefore be interpreted alongside the other indicators rather than treated as standalone proof of malicious activity.

**Evidence:**

![MXToolbox authentication results](../screenshots/email-01/MXtoolbox%20results.png)

## 6. Threat Intelligence and IP Reputation

### 6.1 IP Address: `185.220.101.47`

The supplied AbuseIPDB screenshots show the following information at the time of analysis:

| Attribute | Observed result |
|---|---|
| Abuse reports | 6,841 |
| Confidence of abuse | 100% |
| Classification | Tor exit node |
| Hostname | `tor-exit-47.for-privacy.net` |
| Domain | `for-privacy.net` |
| Location shown | Berlin, Germany |

The reported volume of abuse and 100% confidence score indicate a strong negative reputation in AbuseIPDB at the time of the lookup. The evidence also identifies the IP as a Tor exit node and shows recent abuse reports.

However, the IP reputation result does not independently prove that this address sent the email or that it was responsible for the sign-in activity described in the message.

**Evidence:**

![AbuseIPDB reputation summary for 185.220.101.47](../screenshots/email-01/AbuseIPDB%20analysis%201.png)

![Recent AbuseIPDB reports for 185.220.101.47](../screenshots/email-01/AbuseIPDB%20analysis2.png)

### 6.2 IP Address Mentioned in the Email: `91.108.56.122`

The email body identifies `91.108.56.122` as the IP address associated with the alleged suspicious sign-in.

This should be treated as an **IP claimed by the email**, not as a confirmed source of the sign-in. The message's claim would require independent verification through appropriate Microsoft account logs or other reliable evidence.

The supplied screenshots do not establish that `91.108.56.122` was responsible for sending the email. The distinction between an IP mentioned in the message and an IP established by verified routing or account logs is important for accurate attribution.

## 7. Consolidated Indicators of Concern

| Indicator | Observation | Significance |
|---|---|---|
| Displayed sender | `account-security@microsoft.com` | Claims to represent Microsoft |
| Reply-To | `ms-support@account-ms365-verify.top` | Uses a different domain |
| Return-Path | `noreply@mailer-srv-dispatch.xyz` | Uses another distinct domain |
| Message theme | Account suspension and suspicious sign-ins | Exploits concern about account access |
| Deadline | Verification within 24 hours | Creates urgency |
| Threat intelligence | `185.220.101.47` | 6,841 AbuseIPDB reports and 100% confidence in the supplied evidence |
| IP classification | Tor exit node | Reputation does not identify the sender |
| Email authentication | SPF, DKIM, and DMARC concerns | Reduces confidence in message authentication |
| IP stated in email | `91.108.56.122` | Alleged sign-in IP, not independently verified |

## 8. Key Findings

1. **Possible Microsoft impersonation:** The displayed sender claims to represent Microsoft, while the Reply-To and Return-Path use different domains.
2. **Social-engineering indicators:** The account-suspension warning and 24-hour deadline may pressure the recipient into acting quickly.
3. **Email-authentication concerns:** The supplied MXToolbox results identify SPF and DMARC failures and a DKIM authentication failure, although DKIM alignment is shown as passing.
4. **Negative IP reputation:** AbuseIPDB reported 6,841 abuse reports and 100% confidence for `185.220.101.47` in the supplied evidence.
5. **Attribution limitation:** The reputation result does not prove that `185.220.101.47` sent the email or that `91.108.56.122` caused the alleged sign-in.
6. **No confirmed compromise:** The available evidence does not establish successful credential theft or account compromise.

## 9. Recommended Defensive Actions

- Do not click links or provide credentials through the suspicious email.
- Verify account-security claims by navigating independently to Microsoft's official website.
- Preserve the original email, complete headers, screenshots, and relevant timestamps.
- Search email gateway logs for related messages, domains, and sender addresses.
- Review the sender-domain discrepancies and authentication results.
- Investigate the observed IP addresses through approved threat-intelligence procedures.
- If credentials were submitted, change the password through the official service and notify the appropriate security team.
- If a link or attachment was opened, follow the organisation's incident-response procedures.
- Monitor for similar account-suspension and verification-themed messages.

## 10. Conclusion

The evidence indicates that the email is highly suspicious and consistent with a possible Microsoft impersonation and credential-phishing attempt. The sender-related domain discrepancies, urgency, authentication concerns, and negative reputation of `185.220.101.47` collectively support this assessment.

The investigation does not establish that the observed IP sent the message, that the alleged sign-in occurred as described, or that the recipient's account was compromised.

The recommended disposition is to **treat the email as suspicious, avoid interacting with its links, and independently verify any account-security claim through an official Microsoft channel**.
