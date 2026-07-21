# Overview

### A mid-sized technology company called Compliant Secure, with 50 employees, has recently migrated to Microsoft 365. During a routine security review, the SOC team discovered suspicious authentication patterns.

![Overview](screenshots/OV.png)

<br>

### Methodology:

**Our task is to investigate these authentication anomalies, implement detection mechanisms, and harden the Azure AD environment.**

---

<br>

### Attack Chain:

                                       Reconnaissance of valid Microsoft 365 user accounts
                                                               ↓
                                       Password spray attack launched against 89 user accounts
                                            (ResultType 50126 - Invalid credentials)
                                                               ↓
                                   Authentication attempts distributed across multiple AWS IPs
                                          (3.123.14.162, 3.123.14.126, 3.70.195.178)
                                                               ↓
                                   Azure AD Smart Lockout triggered on targeted victim accounts
                                                 (ResultType 50053 observed)
                                                               ↓
                                  One account successfully authenticated after password spraying
                                      (louisa.hartis@compliantsecure.store compromised)
                                                               ↓
                                Successful Microsoft Entra ID / Azure AD authentication (ResultType 0)
                                                               ↓
                                   Attacker gains authenticated access to Microsoft 365 tenant
                                                               ↓
                              Opportunity for privilege enumeration, persistence, and lateral movement
---

<br>

## Indicators of Compromise:

---

<br>

## MITRE ATT&CK Mapping:

---

<br>

# Investigation:

## 1. Initial Attack Detection

### 1.1) During the initial investigation, you notice a pattern of failed authentication attempts. What is the most common Result Type associated with password spray attacks in Azure AD sign-in logs?

![1.1](screenshots/1.1.0.png)

Outputting the whole table, we see the "ResultType" column, so we can query for the number of distinct result types:

![1.1](screenshots/1.1.1.png)

We see the most frequent result type is 50126 by far
**Answer: 50126**

<br>

### 1.2) Analyzing the sign-in logs, you identify multiple source IP addresses attempting authentication. Which IP address had the highest number of failed login attempts during the attack window?

![1.2](screenshots/1.2.0.png)

![1.2](screenshots/1.2.1.png)

**Answer: 3.123.15.9**

<br>

### 1.3) The attacker appears to be using a specific user agent string across all spray attempts. What is the user agent string identified in the attack?

![1.3](screenshots/1.3.0.png)

**Answer: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/104.0.0.0 Safari/537.36**

<br>

### 1.4) Smart Lockout events are crucial for identifying password spray victims. What is the specific sign-in error code that indicates an account has been locked out by Azure AD Smart Lockout?

reference 1.1

**Answer: 50126**

<br>

### 1.5) What time (UTC) in CreatedDateTime did the password spray attack begin based on the first failed authentication attempt?

![1.5](screenshots/1.5.0.png)

**Answer: 2025-06-29 18:35**

---

<br>

## 2. Attack Pattern Analysis

### 2.1) How many unique user accounts in the Compliant Secure company were targeted in this password spray attack?

![2.1](screenshots/2.1.0.png)

![2.1](screenshots/2.1.1.png)

**Answer: 89**

<br>

### 2.2) The attack originated from a single country. What is the name of the region where the attack originated?

![2.2](screenshots/2.2.0.png)

**Answer: DE**

---

<br>

## 3. Detection and Response

### 3.1) Detection in Microsoft Sentinel relies on analytics rules that continuously monitor logs for suspicious patterns. To investigate this attack: In Microsoft Sentinel, navigate to Content Hub and install the Microsoft Entra ID solution. Once installed, go to Analytics > Rule templates and you'll see several analytics rules related to password spray attacks. What is the name of the analytics rule that identifies evidence of failures from multiple accounts against Microsoft Entra ID applications?

![3.1](screenshots/3.1.0.png)

![3.1](screenshots/3.1.1.png)

**Answer: Password spray attack against Microsoft Entra ID application**

<br>

### 3.2) Create an analytics rule based on the template from the previous question, but modify it to use the "SigninLogs_CL" custom table and update all column references to match the custom table schema. In the rule configuration, there's a parameter called authenticationThreshold that defines how many failed account attempts from a single IP address trigger an alert. Based on the attack patterns observed in this incident, what is the maximum value you should set for authenticationThreshold to ensure the rule would have detected this specific attack?

![3.2](screenshots/3.2.0.png)

![3.2](screenshots/3.2.1.png)

**Answer: 3**

<br>

### 3.3) The analytics rule you created to detect the attack patterns by analyzing failed authentication attempts over specific time periods. Review the KQL query in your analytics rule and locate the parameter that defines the time window for grouping authentication attempts. What is the value (in minutes) set for the authenticationWindow parameter?

![3.3](screenshots/3.3.0.png)

**Answer: 20**

<br>

### 3.4) After creating and enabling your custom analytics rule using SignInLogs only, it successfully triggers and generates an incident for the attack that happened. Navigate to Incidents in Microsoft Sentinel and open the newly created incident. Review the entities section, which shows all IP addresses involved in this attack. List all attacker IP addresses identified in the incident.

![3.4](screenshots/3.4.0.png)

**Answer: 3.123.14.162, 3.123.14.126, 3.70.195.178**

<br>

### 3.5) What is the minimum number of failed attempts from a single IP before Smart Lockout triggers for an unfamiliar location (based on default settings)?

Quick lookup

![3.5](screenshots/3.5.0.png)

**Answer: 10**

---

<br>

## 4. Investigation and Forensics

### 4.1) Investigation revealed that despite the widespread attack, only one account was successfully compromised. What is the user principal name of the account that was successfully breached?

![4.1](screenshots/4.1.0.png)

**Answer: louisa.hartis@compliantsecure.store**

---

<br>

## 5. Mitigation and Controls

### 5.1) What Conditional Access policy would have prevented 99% of this password spray attack according to Microsoft's research?

Quick lookup

![5.1](screenshots/5.1.0.png)

**Answer: Block legacy authentication**

<br>

### 5.2) When implementing Azure AD Password Protection to prevent compromised accounts, what is the maximum number of password entries you can add to the custom banned password list?

![5.2](screenshots/5.2.0.png)

**Answer: 1000**

<br>

### 5.3) For federated environments, what Windows Server 2019 feature provides similar protection to Azure AD Smart Lockout?

![5.3](screenshots/5.3.0.png)

**Answer: Extranet Smart Lockout**

<br>

### 5.4) Analytics rules in Microsoft Sentinel can trigger automated responses through playbooks when incidents are created. This enables immediate remediation actions without manual intervention. In your custom analytics rule configuration, navigate to the "Automated response" tab where you can attach playbooks. If you were to create a playbook that revokes all active sessions for compromised users detected by this rule, what Microsoft Graph API method would the playbook use to invalidate all user sessions?

**Answer:**

<br>

### 5.5) According to NIST guidelines referenced in the mitigation section, what is the recommended minimum password length for modern password policies?

**Answer:**

---

**Completed:**

![done](screenshots/complete.png)
