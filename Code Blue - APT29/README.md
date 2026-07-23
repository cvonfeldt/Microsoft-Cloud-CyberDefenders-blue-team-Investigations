# Overview

**On February 10, 2026, the Security Operations Center (SOC) at Meridian Health, a mid-sized healthcare organization managing outpatient clinics, diagnostic centers, and a small research division, received multiple security alerts indicating suspicious authentication activity.**

**Meridian Health recently modernized their IT infrastructure, migrating critical workloads to Microsoft Azure and adopting cloud-based collaboration tools including Microsoft 365 and Miro for their clinical research teams. As part of their digital transformation, they implemented federated authentication using SAML for single sign-on across their SaaS applications.**

**The incident began when the SOC detected multiple failed authentication attempts against a user account, followed by a successful login from an unfamiliar external IP address. Shortly after, suspicious OAuth consent activity and unauthorized access patterns were detected across Azure resources, Key Vault, and third-party applications.**

**Initial triage suggests the attacker leveraged the initial compromise to pivot through multiple accounts, access sensitive secrets, and potentially exfiltrate protected health information (PHI). The attack pattern indicates sophisticated techniques consistent with advanced persistent threat (APT) activity.**

![Overview](screenshots/OV.png)

<br>

### Methodology:

**My task is to investigate the full scope of this breach using Microsoft Sentinel. The available logs for this investigation include: SignInLogs, AuditLogs, KeyVaultLogs, AzureActivity, StorageBlobLogs, MiroAuditLogs, and OfficeActivity to trace the attacker's movements across Azure and connected applications, identify all compromised accounts, and document the techniques used throughout the attack chain.**

---

<br>

### Attack Chain:


---

<br>

## Indicators of Compromise:


---

<br>

## MITRE ATT&CK Mapping:

---

<br>

# Investigation:

## 1. Initial Access

### 1.1) The attack begins with a password spray targeting Taylor. As the authentication attempts progressed, different error codes were returned. What were the observed error codes, in chronological order?

![1.1](screenshots/1.1.0.png)

**Answer: **

<br>

### 1.2) The password spray eventually found the correct password, but the attacker couldn't complete authentication due to MFA. The attacker then proceeded with a phishing attack. Attackers often use typosquatting to create convincing phishing domains that closely resemble legitimate ones. What is the sender domain used in the phishing email?

![1.2](screenshots/1.2.0.png)

**Answer: **

<br>

### 1.3) Threat intelligence often relies on geographic attribution. What is the country associated with the attacker's first IP address, according to the logs?

![1.3](screenshots/1.3.0.png)

**Answer: **

<br>

### 1.4) What is the name of the phishing technique the attacker used to bypass MFA and gain access?

![1.4](screenshots/1.4.0.png)

**Answer: **

<br>

### 1.5) This phishing technique is particularly dangerous because it uses legitimate Microsoft login pages. However, it has a built-in time limitation. What is the default lifetime (in minutes) before the authentication code expires?

![1.5](screenshots/1.5.0.png)

**Answer: **

<br>

### 1.6) What Entra ID Conditional Access policy setting can block this type of authentication flow?

![1.6](screenshots/1.6.0.png)

**Answer: **

---

<br>

## 2. Post-Compromise: Token Abuse & Initial Discovery

### 2.1) After the initial compromise, the attacker abused OAuth token families (FOCI) to access multiple applications without re-authentication. Excluding the initial phishing application, how many additional applications did the attacker access using this method?

![2.1](screenshots/2.1.0.png)

**Answer: **

<br>

### 2.2) What is the CorrelationId of the first directory enumeration activity performed by the attacker?

![2.2](screenshots/2.2.0.png)

**Answer: **

---

<br>

## 3. Mailbox Exploitation & Credential Discovery

### 3.1) The attacker configured email forwarding on the compromised mailbox for persistent access. To what external email address were the emails forwarded?

![3.1](screenshots/3.1.0.png)

**Answer: **

<br>

### 3.2) What is the name of the malicious inbox rule created by the attacker to hide security alerts and notifications?

![3.2](screenshots/3.2.0.png)

**Answer: **

<br>

### 3.3) During the initial reconnaissance within the user's account, how many emails were accessed and how many files were downloaded?

![3.3](screenshots/3.3.0.png)

**Answer: **

<br>

### 3.4) While searching through the victim's emails and files, the attacker discovered an Excel file containing credentials for a service account. What is the UPN of this first compromised service account?

![3.4](screenshots/3.4.0.png)

**Answer: **

---

<br>

## 4. Privilege Escalation & Lateral Movement

### 4.1) After noticing suspicious activity, the victim attempted to secure their account. At what time (ActivityDateTime) did the victim change their password?

![4.1](screenshots/4.1.0.png)

**Answer: **

<br>

### 4.2) The attacker switched infrastructure when pivoting to the first compromised service account. What IP address was used to authenticate as this service account?

![4.2](screenshots/4.2.0.png)

**Answer: **

<br>

### 4.3) Using the first compromised service account, the attacker enumerated Azure Automation resources. What are the names of the Automation Accounts discovered?

![4.3](screenshots/4.3.0.png)

**Answer: **

<br>

### 4.4) The attacker examined job outputs from one of the Automation Accounts and discovered credentials for another service account. What is the password that was exposed in the job output?

![4.4](screenshots/4.4.0.png)

**Answer: **

<br>

### 4.5) The attacker continued enumerating Azure resources and found credentials for a third service account in the deployment history. What is the UPN of this third compromised service account?

![4.5](screenshots/4.5.0.png)

**Answer: **

---

<br>

## 5. High-Value Asset Discovery

### 5.1) The attacker discovered that the third service account has Key Vault access permissions and proceeded to access it. What is the name of the Key Vault that was successfully accessed?

![5.1](screenshots/5.1.0.png)

**Answer: **

<br>

### 5.2) The attacker extracted multiple secrets from the Key Vault. What is the name of the secret containing the SAML signing certificate?

![5.2](screenshots/5.2.0.png)

**Answer: **

<br>

### 5.3) Among the secrets stored in the Key Vault, what is the name of the secret that contains the password for the fourth service account credentials?

![5.3](screenshots/5.3.0.png)

**Answer: **

---

<br>

## 6. Federation Compromise

### 6.1) With the SAML signing certificate and its password, the attacker can forge authentication tokens to impersonate any user. What are the usernames (without domain) of the two users impersonated via Silver SAML?

![6.1](screenshots/6.1.0.png)

**Answer: **

<br>

### 6.2) At what time (UTC) did the first Silver SAML impersonation login occur?

![6.2](screenshots/6.2.0.png)

**Answer: **

<br>

### 6.3) Identity federation abuse is a common post-compromise technique used to bypass authentication controls. What is the MITRE ATT&CK technique ID for forging SAML tokens?

![6.3](screenshots/6.3.0.png)

**Answer: **

---

<br>

## 7. Data Exfiltration: Collaboration Platform

### 7.1) During the Miro exfiltration phase, a third IP address is observed in the activity logs. What is the associated User-Agent string for that IP address?

![7.1](screenshots/7.1.0.png)

**Answer: **

<br>

### 7.2) How many unique Miro boards were exported in total?

![7.2](screenshots/7.2.0.png)

**Answer: **

---

<br>

## 8. Data Exfiltration: Cloud & Hybrid Infrastructure

### 8.1) During the Azure enumeration phase, the attacker identified an SQL server and enumerated its databases. What are the names of the discovered databases?

![8.1](screenshots/8.1.0.png)

**Answer: **

<br>

### 8.2) The attacker enumerated Azure Arc hybrid machines. What are the names of all Arc machines discovered?

![8.2](screenshots/8.2.0.png)

**Answer: **

<br>

### 8.3) The attacker switched to a fourth IP address for additional operations. What is this IP address?

![8.3](screenshots/8.3.0.png)

**Answer: **

<br>

### 8.4) What is the name of the storage account targeted for data exfiltration?

![8.4](screenshots/8.4.0.png)

**Answer: **

<br>

### 8.5) What are the names of all containers accessed by the attacker, ordered from highest to lowest based on the number of logged operations?

![8.5](screenshots/8.5.0.png)

**Answer: **

<br>

### 8.6) During the exfiltration phase, the attacker generated a SAS token. What is the expiration date and what permissions were granted on this token?

![8.6](screenshots/8.6.0.png)

**Answer: **

<br>

### 8.7) User Delegation SAS tokens allow delegated access to Azure Blob Storage resources without sharing account keys. These tokens are time-limited to reduce risk of misuse. What is the maximum validity period (in days) for a User Delegation SAS token?

![8.7](screenshots/8.7.0.png)

**Answer: **

---

<br>

## 9. Persistence: Application Backdoors

### 9.1) The attacker established persistence by adding backdoor credentials and escalating privileges on multiple OAuth applications. What are the display names of the applications modified, in chronological order?

![9.1](screenshots/9.1.0.png)

**Answer: **

<br>

### 9.2) After adding backdoor credentials to multiple applications, the attacker discovered that one of them had existing permissions for Microsoft 365 data and used it to access sensitive data. How many MailItemsAccessed and FileDownloaded events were logged through this backdoor application?

![9.2](screenshots/9.2.0.png)

**Answer: **

---

<br>

## 10. Threat Intelligence & Remediation

### 10.1) Understanding MITRE ATT&CK techniques helps map attacker behaviors. Which MITRE ATT&CK technique ID corresponds to abusing OAuth tokens or the device code flow to access applications without needing user credentials?

![10.1](screenshots/10.1.0.png)

**Answer: **

<br>

### 10.2) Email rules abuse is a common method attackers use to maintain persistence and evade detection. What is the MITRE ATT&CK technique ID for creating Email Forwarding Rules?

![10.2](screenshots/10.2.0.png)

**Answer: **

<br>

### 10.3) OAuth application compromise can allow attackers to access sensitive data without user credentials. Which MITRE ATT&CK technique ID corresponds to adding credentials to OAuth applications?

![10.3](screenshots/10.3.0.png)

**Answer: **

<br>

### 10.4) What Azure Key Vault diagnostic setting category must be enabled to log all secret access attempts?

![10.4](screenshots/10.4.0.png)

**Answer: **

<br>

### 10.5) Instead of storing credentials in Automation runbooks or deployment templates, what Azure feature should be used to eliminate the need for stored secrets?

![10.5](screenshots/10.5.0.png)

**Answer: **

---

<br>

## Completed:

![done](screenshots/complete.png)
