AI Phishing Detection Agent (Gmail + LLM + Automation)

An autonomous email security agent that detects phishing attempts in real-time using LLM-based analysis, confidence scoring, and multi-step decision workflows.

🚀 Overview

This project implements a multi-stage phishing detection pipeline that:

Monitors incoming Gmail emails
Filters trusted senders (allowlist)
Uses an LLM to analyze phishing indicators
Classifies emails with verdict + confidence score
Automatically triggers alerts or labels based on risk level

The system is designed to reduce false positives while maintaining strong detection capability.

🧠 Core Features
✅ Real-time Gmail monitoring
✅ Trusted domain allowlist (cost + noise reduction)
✅ LLM-based phishing detection (Claude Sonnet)
✅ Structured output: verdict, confidence, reason
✅ Confidence-based decision routing
✅ Automated alerting system
✅ Gmail labeling for triage (Phishing, Suspicious)
✅ False-positive control via thresholding
⚙️ System Architecture
New Email (Gmail Trigger)
        │
        ▼
Allowlist Filter (Trusted Domains)
        │
        ▼
LLM Analysis (Phishing Detection)
        │
        ▼
Decision Engine (Paths)
   ├── High Confidence Phishing → Alert + Label
   ├── Suspicious / Low Confidence → Warning + Label
   └── Legitimate → Mark as Read
🔍 Detection Logic

The LLM evaluates emails across multiple dimensions:

Sender authenticity (spoofing, domain mismatch)
Header anomalies (SPF/DKIM/DMARC signals)
Social engineering tactics (urgency, threats)
Malicious links (URL mismatch, shorteners, IP links)
Content quality (grammar, tone, impersonation)
Attachment risk (executables, macros, archives)
📊 Classification Strategy
Verdict	Confidence	Action
Phishing	≥ 61	🚨 Alert + Label "Phishing"
Suspicious	Any	⚠️ Warning + Label "Suspicious"
Phishing	< 61	⚠️ Treated as Suspicious
Legitimate	Any	✅ Mark as Read
⚡ Key Design Decisions
1. Allowlist Filtering

Skips trusted domains to:

Reduce API cost
Improve performance
Minimize unnecessary analysis
2. Confidence Thresholding
Prevents alert fatigue
Separates high-risk vs uncertain threats
Improves usability in real-world scenarios
3. Structured LLM Output

Instead of free text:

{
  "verdict": "phishing | suspicious | legitimate",
  "confidence": 0-100,
  "reason": "explanation"
}

This ensures reliable automation and deterministic routing.

4. First Email Trigger Only

Avoids duplicate alerts from reply threads.

🧪 Testing

Tested with realistic scenarios:

Legitimate personal email → correctly ignored
Fake login alert → detected as phishing
Fake invoice scam → detected as phishing
Results
✅ Accurate classification across all test cases
⚡ Response time: <10 seconds
🔁 Stable workflow execution
🔐 Security & Ethics
Processes only required email content
Designed as assistive security layer, not a replacement for user judgment
Acknowledges LLM limitations (false positives/negatives possible)
🔮 Future Improvements
Threat intelligence integration (VirusTotal, Safe Browsing)
Sender authentication validation (SPF/DKIM/DMARC parsing)
Feedback loop for continuous improvement
Dashboard for monitoring and analytics
Chrome extension / inbox UI integration
📌 Author

Varenya Sawant
Cybersecurity | AI Security Automation | Building practical security systems