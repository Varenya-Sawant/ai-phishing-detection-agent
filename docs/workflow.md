Phishing Email Detector — Workflow Specification
Version: Current Published | Platform: Relay.app | Integration: Gmail

Overview
An autonomous, self-learning email security pipeline that triages every incoming Gmail message through a multi-layer decision engine — combining deterministic rule-based filtering, institutional memory (sender blocklist/allowlist), domain intelligence, and AI-powered forensic analysis — to classify, label, archive, and learn from phishing threats with minimal human intervention.

Architecture Summary
Trigger → Pre-filters → Memory Lookup → Domain Intel → AI Analysis → Routing → Actions + Logging
Three persistent data stores back the system:

Table	Purpose
Known Bad Senders	Block list of confirmed phishing senders
Known Good Senders	Allow list of confirmed legitimate senders
Phishing Analysis Log	Audit trail of every email processed


Stage 1 — Trigger
Trigger: Gmail — Email Received

Fires on every new inbound email
Scoped to first message in thread only to avoid re-processing replies
Stage 2 — Deterministic Pre-Filters
Step 2 · Extract Domain & Flag Attachments
Two parallel transforms run on the raw email:

Sender Domain — extracts and lowercases the domain from the sender's email address
Dangerous Attachment Count — filters attachments by known-malicious extensions and returns the count
Flagged extensions: .exe .js .bat .scr .docm .xlsm .vbs .cmd .iso .lnk .html .htm .jar .ps1

Step 3 · Skip Trusted Senders
Hard-coded allowlist check. If the sender domain exactly Is same list as any of the following, the run ends immediately — no further processing or AI cost incurred:

google.com · github.com · microsoft.com · apple.com · amazon.com · linkedin.com · slack.com · notion.so · stripe.com

Stage 3 — Institutional Memory Lookup
Steps 4 and 5 run sequentially to check the workflow's learned sender history Before invoking any AI.

Step 4 · Check Known Bad Senders
Queries the Known Bad Senders table for an exact match on sender email. Returns a list (up to 50 records).

Step 5 · Check Known Good Senders
Queries the Known Good Senders table for an exact match on sender email. Returns a list (up to 50 records).

Stage 4 — Domain Intelligence
Step 6 · RDAP Domain Age Lookup
Makes a live HTTP GET request to rdap.org using the sender's domain. Retrieves domain registration events. Continues on error (graceful degradation for domains that don't return RDAP data).

Step 7 · Parse Domain Age
Custom code parses the RDAP response and computes:

ageDays — number of days since domain registration (-1 if unknown)
registrationDate — ISO date string
ageBucket — risk tier: new / recent / established / old / unknown
Stage 5 — Memory Short-Circuit (Paths)
Step 8 · Memory Short-Circuit
Evaluates the results from Steps 4 & 5. Takes the first matching path:

╔ Path A — 🚫 Known Bad Sender (Auto-Block)
Condition: Known Bad Senders result is not empty

Step	Action
Step 9	Send alert email to  with full context
Step 10	Apply "Phishing" label to email
Step 11	Archive the email
Step 12	Log to Phishing Analysis Log (verdict: known-bad-skip, confidence: 100)
Step 13	End Run


⚡ AI analysis is skipped entirely — zero AI credits consumed.

╔ Path B — ✅ Known Good Sender (Auto-Allow)
Condition: Known Good Senders result is not empty

Step	Action
Step 14	Mark email as read
Step 15	Apply "Safe" label to email
Step 16	Log to Phishing Analysis Log (verdict: known-good-skip, confidence: 100)
Step 17	End Run


⚡ AI analysis is skipped entirely — zero AI credits consumed.

╔ Path C — Unknown Sender
Condition: No match in either memory table → proceeds to AI analysis

(No actions — flow continues to Stage 6)

Stage 6 — AI Forensic Analysis
Step 18 · AI Prompt — Claude Haiku 4.5
Model: claude-haiku-4-5 (via Relay.app AI credits)
Output mode: Structured object

Data passed to the model:

Domain age & risk bucket (Step 7)
Sender name & email address
Email subject
Email body (plain text)
Raw headers
Send date/time
Email URL
Gmail labels
Is first in conversation flag
Has external recipients flag
Sender domain & dangerous attachment count (Step 2)
Evaluation dimensions:

Sender authenticity (display name vs address, lookalike domains)
Header authentication (SPF, DKIM, DMARC signals)
Social engineering (urgency, fear, authority impersonation)
Link analysis (anchor vs URL mismatch, obfuscation, suspicious TLDs)
Content quality (generic greetings, unnatural phrasing, scam patterns)
Brand impersonation
Attachment risk (beyond the pre-computed count)
Structured Output:

Field	Type	Description
verdict	enum	phishing / suspicious / legitimate
confidence	integer	0–100 score
reason	string	2–5 sentence technical explanation


Stage 7 — Verdict Routing
Step 19 · Select a Path
╔ Path 1 — 🚨 Phishing (High Confidence) or Dangerous Attachment
Conditions (either):

verdict = phishing AND confidence ≥ 75
OR dangerous attachment count > 0
Step	Action
Step 20	Send phishing alert email (verdict, confidence, reason, full email details)
Step 21	Apply "Phishing" label
Step 22	Archive the email
Step 23	Add sender to Known Bad Senders table (source: ai-phishing)
Step 24	Log to Phishing Analysis Log (verdict: phishing)


╔ Path 2 — ⚠️ Suspicious / Low-Confidence Phishing
Conditions (either):

verdict = suspicious
OR verdict = phishing AND confidence < 75
Step	Action
Step 25	Send suspicious warning email
Step 26	Apply "Suspicious" label
Step 27	Log to Phishing Analysis Log (verdict: suspicious, user_feedback: pending)
Step 28	Send human feedback request via email (7-day deadline)


Step 29 · Apply User Feedback — branches on the user's response:

Response	Action
Confirmed Phishing	Step 30: Add to Known Bad Senders (source: user-feedback)
Legitimate	Step 31: Add to Known Good Senders (source: user-feedback)
Not Sure	No action taken


🧠 This is the system's active learning loop — human corrections improve future auto-routing accuracy.

╔ Path 3 — ✅ Legitimate
Condition: verdict = legitimate

Step	Action
Step 32	Mark email as read
Step 33	Apply "Safe" label
Step 34	Add sender to Known Good Senders (source: ai-legitimate)
Step 35	Log to Phishing Analysis Log (verdict: legitimate)


Logging Schema
Every execution path writes a record to the Phishing Analysis Log with:

Field	Description
Sender Email / Domain	Who sent it
Subject	Email subject line
Verdict	Final classification
Confidence	AI confidence score (or 100 for memory-based decisions)
AI Reason	Explanation of the verdict
Dangerous Attachments	Count of flagged file types
Domain Age Days	Days since domain registration
Path Taken	Which routing branch executed
User Feedback	correct / false-positive / false-negative / pending
Email URL	Direct link to the email in Gmail


Key Design Principles
Principle	Implementation
Cost efficiency	Memory short-circuit skips AI for known senders — zero credits consumed
Defense in depth	Three independent filter layers Before AI is invoked
Graceful degradation	RDAP failures return unknown and don't block the pipeline
Active learning	User feedback on suspicious emails continuously trains the blocklist/allowlist
Full auditability	Every email processed is logged regardless of verdict
Conservative bias	AI prompt defaults to more conservative verdict when signals are mixed


