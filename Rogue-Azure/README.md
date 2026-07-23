# Overview

###  On November 14, 2025, security monitoring detected suspicious authentication activity in the Azure tenant, with anomalous sign-in patterns from multiple geographic locations. Shortly after, automated alerts flagged unauthorized administrative actions and configuration changes within the environment.

![Overview](screenshots/OV.png)

<br>

### Methodology:

**Provided with Azure sign-in logs, audit logs, and storage access logs from the affected tenant, my mission is to investigate the incident, determine how the attacker gained initial access, identify what persistence mechanisms were established, document any privilege changes, and confirm whether sensitive data was accessed or exfiltrated.**

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

### 1.1) The investigation begins by analyzing a password spray attack that targeted several users in the primary tenant. What IP address did the attacker originate the password spray attack from?

![1.1](screenshots/1.1.0.png)

![1.1](screenshots/1.1.1.png)

**Answer: 52.59.240.166**

<br>

### 1.2) After numerous failed attempts, the attacker successfully gained access to an account. What is the username of the first account that was compromised?

![1.2](screenshots/1.2.0.png)

**Answer: mharmon@compliantsecure.store**

---

<br>

## 2. Command and Control

### 2.1) Following the initial compromise, the attacker began using a new infrastructure for post-exploitation activities. What is the second IP address used by the attacker?

![2.1](screenshots/2.1.0.png)

**Answer: 52.221.180.165**

<br>

### 2.2) From which country did the successful sign-in originate when the attacker pivoted to their secondary infrastructure for post-exploitation activities?

![2.2](screenshots/2.2.0.png)

**Answer:**

---

<br>

## 3. Persistence

### 3.1) To establish persistence, the attacker registered malicious applications. What is the name of the first application they created?

![3.1](screenshots/3.1.0.png)

**Answer:**

<br>

### 3.2) The attacker created a second application to ensure persistent access, this one intended to access directory information. What is the name of this second application?

See 3.1

**Answer: VaultApp**

---

<br>

## 4. Privilege Escalation

### 4.1) To create a redundant backdoor, the attacker used the compromised administrator account to elevate the privileges of another user. What is the User Principal Name of the account that had its privileges escalated?

![4.1](screenshots/4.1.0.png)

![4.1](screenshots/4.1.1.png)

**Answer: lwilliams@compliantsecure.store**

<br>

### 4.2) What highly privileged role was assigned to the second user account to grant it administrative control over the tenant?

![4.2](screenshots/4.2.0.png)

**Answer: Global Administrator**

---

<br>

## 5. Collection & Exfiltration

### 5.1) The attacker's final objective was data exfiltration. They targeted a specific storage resource to access sensitive files. What is the name of the storage account they accessed?

![5.1](screenshots/5.1.0.png)

**Answer: mainstoragestore01**

<br>

### 5.2) The attacker successfully downloaded a sensitive file from the storage account. What is the name of the exfiltrated file?

See 5.1

**Answer: Confidintal.png**

---

<br>

**Completed:**

![done](screenshots/complete.png)

