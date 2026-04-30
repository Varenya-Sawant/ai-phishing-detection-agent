# Phishing Detection Logic

## Overview

This document defines the logic used to evaluate emails for phishing risk.
The system analyzes multiple categories of signals and determines a classification based on their strength and combination.

---

## Signal Categories

All categories must be evaluated for every email.

### 1. Sender Analysis

* Display name vs email mismatch
* Lookalike or spoofed domains
* Free email providers used for business claims

---

### 2. Header Indicators

* SPF, DKIM, DMARC failures or inconsistencies
* Suspicious or unusual relay paths
* IP/geolocation inconsistencies

> Missing authentication increases risk but is not definitive proof of phishing.

---

### 3. Social Engineering Signals

* Urgency or pressure tactics ("act now")
* Threats ("account will be locked")
* Requests for credentials, OTPs, or sensitive data
* Financial requests (gift cards, transfers)
* Authority impersonation

---

### 4. Link Analysis

* Mismatched anchor text vs actual URL
* URL shorteners or obfuscation
* Lookalike domains
* IP-based URLs
* Redirect or tracking-heavy links

---

### 5. Content Signals

* Poor grammar or unnatural phrasing
* Generic greetings
* Fake alerts or warnings
* Prize, refund, or scam patterns
* Account verification requests

---

### 6. Brand Impersonation

* Claims of trusted brands (banks, tech, government)
* Visual or textual imitation

---

### 7. Attachment Risk

* Executable files
* Macro-enabled documents
* Password-protected archives
* Unexpected attachments

---

## Decision Logic

### Signal Prioritization

* Technical indicators (links, headers, attachments) carry more weight than language-based signals

---

### Strong Indicators Rule

* A single high-confidence malicious signal (e.g., credential harvesting, malicious link) can justify a phishing classification

---

### Signal Combination

* Multiple weak or moderate signals across categories increase overall risk
* Combined signals may elevate classification to phishing even without a single dominant indicator

---

### Ambiguity Handling

* Mixed or incomplete signals result in a suspicious classification
* When uncertain, bias toward higher risk

---

## Scoring Model

Confidence score reflects strength of observed signals:

* **0–30** → Weak indicators
* **31–60** → Moderate suspicion
* **61–80** → Strong phishing indicators
* **81–100** → Highly likely phishing

---

## Classification Mapping

* **phishing**

  * Clear malicious intent
  * Credential harvesting, malware, or fraud indicators
  * Strong technical + social engineering signals

* **suspicious**

  * No definitive exploit but multiple warning signs
  * Inconsistent sender or domain signals
  * Ambiguous or unusual requests

* **legitimate**

  * No significant risk signals
  * Consistent sender, domain, and intent
  * No manipulation or deception patterns

---

## Key Insight

Effective phishing detection relies on:

* Correlating signals across multiple categories
* Prioritizing technical evidence over surface-level content
* Applying conservative classification to reduce missed threats
