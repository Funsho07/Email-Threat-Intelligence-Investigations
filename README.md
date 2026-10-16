# Harmony-Health-Threat-Intelligence-Investigation
Threat Intelligence 
# Harmony Health Breach Investigation — Threat Intelligence & OSINT
# Phishing Email Investigation Report

## Harmony Health Breach Investigation — Threat Intelligence & OSINT

## 1. Executive Summary

This investigation analysed a suspicious email impersonating the Microsoft Account Team. The email claimed that the recipient's Microsoft account had been suspended following suspicious sign-in attempts and urged the recipient to verify their account within 24 hours.

The investigation examined the email's content, sender information, message headers, authentication results, originating IP address, and external threat-intelligence data.

Several indicators raised suspicion, including urgency-based language, a generic greeting, a mismatch between the apparent sender and reply-to address, email-authentication concerns, and an originating IP address associated with substantial abuse reporting.

**Assessment:** The email is highly suspicious and consistent with a phishing attempt designed to pressure the recipient into interacting with a fraudulent account-verification process. The available evidence supports this assessment, although the investigation does not independently establish whether the email successfully compromised an account.

## 2. Investigation Objectives

- Identify indicators of phishing and email impersonation.
- Analyse email headers and authentication results.
- Investigate the originating IP address using threat-intelligence resources.
- Correlate findings from multiple sources to assess the email's credibility.
- Document findings and recommend appropriate defensive actions.

## 3. Tools Used

- **Mozilla Thunderbird:** Email inspection and header analysis.
- **AbuseIPDB:** IP reputation and abuse-report investigation.
- **MXToolbox:** Email-header and authentication analysis.
- **Email headers and message content:** Primary evidence for identifying suspicious indicators.

## 4. Email Analysis

### 4.1 Subject and Message Content

**Subject:** `[Action Required] Your Microsoft Account Has Been Suspended - Verify Now`

The message claimed that suspicious sign-in attempts had been detected and that the recipient's account was temporarily suspended. It threatened loss of access to email and OneDrive files, Microsoft 365 subscription cancellation, and permanent account deletion if verification was not completed within 24 hours.

**Indicators identified:**

- **Urgency and pressure:** The subject and message demanded immediate action.
- **Threat of consequences:** The recipient was warned of account suspension and deletion.
- **Generic greeting:** The message addressed the recipient as "Dear Account User" rather than using a specific name.
- **Verification call to action:** The email encouraged the recipient to verify their account through a prominent button.

These characteristics are consistent with common social-engineering techniques used to encourage recipients to act before verifying a message's legitimacy.

### 4.2 Sender and Reply-To Analysis

The email headers contained the following information:

| Field | Observed Value |
|---|---|
| Display name | Microsoft Account Team |
| From address | `account-security@microsoft.com` |
| Reply-To address | `ms-support@account-ms365-verify.top` |
| Return-Path | `noreply@mailer-srv-dispatch.xyz` |
| Delivered-To | `analyst@targetcorp.com` |

The apparent Microsoft sender identity did not align with the reply-to address or return-path domain.

The reply-to address used the domain `account-ms365-verify.top`, while the return-path used `mailer-srv-dispatch.xyz`. These differences are suspicious in a message claiming to be an official Microsoft account-security notification.

The sender display name alone does not establish authenticity. The differing domains warrant further investigation and are a significant indicator of possible impersonation.

### 4.3 Email Header and Authentication Analysis

The message headers included authentication results and additional metadata that raised concerns about the message's legitimacy.

**Recorded findings:**

- **DKIM:** The header evidence indicated a DKIM failure.
- **SPF and alignment:** The authentication results and MXToolbox analysis indicated authentication or alignment concerns requiring consideration.
- **Anti-abuse metadata:** The message contained an `X-AntiAbuse` header stating that it had been added to track abuse and requesting that it be included in abuse reports.
- **Mailing software:** The `X-Mailer` header identified PHPMailer 5.2.27.

The use of PHPMailer is not inherently malicious, since legitimate applications also use it. However, when considered alongside the suspicious domains, impersonated sender identity, and other indicators, it provides additional context for the investigation.

Email-authentication results should be interpreted carefully: a failed authentication check is a warning indicator, but it does not independently prove that a message is phishing.

## 5. Threat Intelligence and OSINT

### 5.1 Originating IP Address

The email headers identified the originating IP address as:

`185.220.101.47`

This address was investigated using AbuseIPDB.

**Captured AbuseIPDB results:**

- **Total abuse reports:** 6,841
- **Confidence of Abuse:** 100%
- **IP classification:** Tor exit node
- **Reported network usage:** Tor exit traffic
- **Reported location:** Berlin, Germany
- **Hostname:** `tor-exit-47.for-privacy.net`
- **Domain:** `for-privacy.net`

The screenshots also showed recent abuse reports associated with activities such as scanning and brute-force attempts.

These findings indicate that the IP address had a substantial history of reported abuse and was identified as a Tor exit node at the time reflected in the evidence.

However, a Tor exit node's association with abuse reports does not independently establish who sent the email or prove that the IP address was responsible for every reported activity. The result is supporting evidence rather than conclusive attribution.

### 5.2 MXToolbox Analysis

MXToolbox was used to examine the email headers and authentication information.

The captured results highlighted concerns involving email authentication and alignment, supporting further scrutiny of the message's claimed sender identity.

These results were considered alongside the header discrepancies and AbuseIPDB findings rather than being treated as standalone proof of malicious intent.

## 6. Consolidated Indicators of Compromise (IOCs)

| Indicator | Observed Evidence | Security Significance |
|---|---|---|
| Reply-to domain | `account-ms365-verify.top` | Suspicious domain inconsistent with the claimed Microsoft identity |
| Return-path domain | `mailer-srv-dispatch.xyz` | Additional domain requiring investigation |
| Originating IP | `185.220.101.47` | Associated with extensive abuse reporting and identified as a Tor exit node |
| Email authentication | DKIM failure and authentication/alignment concerns | Raises questions about message authenticity |
| Message subject | Account suspension and urgent verification | Uses urgency to influence recipient behaviour |
| Message content | Threats of account deletion and loss of services | Creates pressure to comply |
| Sender presentation | Microsoft Account Team | Display identity does not by itself establish authenticity |

## 7. Investigation Findings

The investigation identified several mutually reinforcing indicators of possible phishing:

1. **Impersonation:** The email presented itself as a Microsoft security notification, but its reply-to and return-path domains differed from the claimed identity.
2. **Social engineering:** The message used urgency, threats, and a generic greeting to encourage immediate action.
3. **Suspicious infrastructure:** The originating IP had a significant history of abuse reports and was identified as a Tor exit node.
4. **Authentication concerns:** The available header evidence included a DKIM failure and other authentication/alignment concerns.
5. **Corroborating evidence:** Findings from the email headers, AbuseIPDB, and MXToolbox collectively increased the likelihood that the message was fraudulent.

The investigation did not establish whether the recipient clicked the verification button, submitted credentials, or experienced an actual account compromise.

## 8. Recommended Actions

- Do not click the verification button or reply to the suspicious message.
- Report the email to the appropriate security or incident-response team.
- Block or investigate the suspicious domains and URLs through approved security controls, after validating the indicators.
- Search email and security logs for related messages, recipient interactions, and connections to the identified indicators.
- If credentials were submitted:
  - Reset the affected password.
  - Revoke active sessions.
  - Review multifactor authentication.
- Monitor for related phishing messages and suspicious account activity.
- Preserve the original email and headers for further investigation.

## 9. Conclusion

The combined evidence supports classifying the email as **highly suspicious and consistent with a phishing attempt impersonating Microsoft**.

The strongest indicators were the inconsistent sender and reply-to domains, urgency-based messaging, email-authentication concerns, and the originating IP address's reported abuse history.

This investigation demonstrates how email-header analysis, threat intelligence, and OSINT can be combined to assess a suspected phishing message. It also highlights the importance of corroborating multiple indicators rather than relying on a single reputation result.

The investigation supports a phishing assessment, but further evidence would be required to confirm credential theft, account compromise, or a successful breach of Harmony Health.

---

## 10. Supporting Evidence

### Phishing Email Sample

![Phishing email](screenshots/Phish%20email.png)

### Email Header Analysis

![Email header analysis](screenshots/email%20header%20analysis.png)

### AbuseIPDB IP Reputation

![AbuseIPDB IP reputation summary](screenshots/AbuseIPDB%20analysis%201.png)

### AbuseIPDB Recent Reports

![AbuseIPDB recent reports](screenshots/AbuseIPDB%20analysis2.png)

### MXToolbox Authentication Results

![MXToolbox results](screenshots/MXtoolbox%20results.png)
