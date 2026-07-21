# Overview

###  A few users in your organization have recently gained access to sensitive resources through what appears to be legitimate Azure AD group membership. The security team flagged an anomaly: a dynamic group seems to have been quietly reconfigured. There's no clear breach, but the group membership rules don’t match what they used to be.

![Overview](screenshots/OV.png)

<br>

### Methodology:

**Using Azure AD logs, Microsoft Sentinel, and Graph API traces, we are tasked with piecing together the timeline, identifying what changed, who made it, and determining whether this was an administrative misstep or a calculated privilege escalation attempt using dynamic groups.**

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

### 1.1) While reviewing Microsoft Sentinel logs during an investigation into suspicious changes, you pivot to Exchange message-trace for the lure. Which email Subject line confirmed the phishing message that kicked off the attack?

![1.1](screenshots/1.1.0.png)

![1.1](screenshots/1.1.1.png)

**Answer:**

<br>

### 1.2) Correlating mail delivery with Azure AD logs, you isolate the first attacker log-on. List the authentication protocol and client application that reveal abuse of the device-code flow.

![1.2](screenshots/1.2.0.png)

![1.2](screenshots/1.2.1.png)

**Answer:**

<br>

### 1.3) Geo-enrichment shows the sign-in came from an unfamiliar location. What public IP address presented the stolen token?

![1.3](screenshots/1.3.0.png)

**Answer:**

<br>

### 1.4) Precise timing is critical for scoping the blast radius. What UTC timestamp marks the attacker’s first successful device-code sign-in?

**Answer:**

---

<br>

## 2. Persistence

### 2.1) Minutes later, logs show a new external identity being invited. What guest UPN did the attacker create for persistence?

![2.1](screenshots/2.1.0.png)

![2.1](screenshots/2.1.1.png)

**Answer:**

---

<br>

## 3. Privilege Escalation

### 3.1) Still tracking the guest’s lifecycle, you notice it being ‘groomed’ to satisfy a dynamic-group rule. List the two attributes and their new values set on the guest account.

![3.1](screenshots/3.1.0.png)

![3.1](screenshots/3.1.1.png)

**Answer:**

<br>

### 3.2) Seconds later, the dynamic-group engine fires. Provide the group name that auto-enrolled the guest and the application identity recorded as the actor.

![3.2](screenshots/3.2.0.png)

![3.2](screenshots/3.2.1.png)

**Answer:**

---

**Completed:**

![done](screenshots/complete.png)
