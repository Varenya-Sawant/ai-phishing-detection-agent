AI PHISHING DETECTION AGENT (GMAIL + LLM + AUTOMATION)

An autonomous email security agent that detects phishing attempts in real-time using LLM-based analysis, confidence scoring, and multi-step decision workflows.

OVERVIEW

This project implements a multi-stage phishing detection pipeline that:

Monitors incoming Gmail emails
Filters trusted senders (allowlist)
Uses an LLM to analyze phishing indicators
Classifies emails with verdict and confidence score
Automatically triggers alerts or labels based on risk level

The system is designed to reduce false positives while maintaining strong detection capability.

CORE FEATURES

Real-time Gmail monitoring
Trusted domain allowlist (cost and noise reduction)
LLM-based phishing detection (Claude Sonnet)
Structured output: verdict, confidence, reason
Confidence-based decision routing
Automated alerting system
Gmail labeling for triage (Phishing, Suspicious)
False-positive control via thresholding

SYSTEM ARCHITECTURE

New Email (Gmail Trigger)
↓
Allowlist Filter (Trusted Domains)
↓
LLM Analysis (Phishing Detection)
↓
Decision Engine

→ High Confidence Phishing → Alert + Label  
→ Suspicious / Low Confidence → Warning + Label  
→ Legitimate → Mark as Read  

DETECTION LOGIC

The LLM evaluates emails across multiple dimensions:

Sender authenticity (spoofing, domain mismatch)
Header anomalies (SPF/DKIM/DMARC signals)
Social engineering tactics (urgency, threats)
Malicious links (URL mismatch, shorteners, IP links)
Content quality (grammar, tone, impersonation)
Attachment risk (executables, macros, archives)

CLASSIFICATION STRATEGY

Verdict: Phishing
Confidence: 61 or higher
Action: Alert and label as "Phishing"

Verdict: Suspicious
Confidence: Any
Action: Warning and label as "Suspicious"

Verdict: Phishing
Confidence: Below 61
Action: Treated as "Suspicious"

Verdict: Legitimate
Confidence: Any
Action: Mark as read

KEY DESIGN DECISIONS

Allowlist Filtering
Skips trusted domains to reduce API cost, improve performance, and minimize unnecessary analysis.
Confidence Thresholding
Prevents alert fatigue, separates high-risk from uncertain threats, and improves usability.

Structured LLM Output
Instead of free text, the system uses structured output:

verdict: phishing | suspicious | legitimate
confidence: 0–100
reason: explanation

This ensures reliable automation and deterministic routing.

First Email Trigger Only
Avoids duplicate alerts from reply threads.

TESTING

Tested with realistic scenarios:

Legitimate personal email → correctly ignored
Fake login alert → detected as phishing
Fake invoice scam → detected as phishing

Results:

Accurate classification across all test cases
Response time under 10 seconds
Stable workflow execution

SECURITY AND ETHICS

Processes only required email content
Designed as an assistive security layer, not a replacement for user judgment
Acknowledges LLM limitations (false positives and negatives are possible)

FUTURE IMPROVEMENTS

Threat intelligence integration (VirusTotal, Safe Browsing)
Sender authentication validation (SPF/DKIM/DMARC parsing)
Feedback loop for continuous improvement
Dashboard for monitoring and analytics
Chrome extension or inbox UI integration

AUTHOR

Varenya Sawant
Cybersecurity | AI Security Automation | Building practical security systems
