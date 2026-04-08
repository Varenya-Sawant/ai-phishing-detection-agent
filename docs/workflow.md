# Workflow Breakdown

## Step 1: Trigger
- Gmail trigger activates on new email
- Only first email in thread is processed

## Step 2: Allowlist Filtering
- If sender domain matches trusted list → stop execution

## Step 3: AI Analysis
- Full email passed to LLM
- Evaluates:
  - Sender authenticity
  - Social engineering signals
  - Links and URLs
  - Content quality
  - Attachments

## Step 4: Classification Output
Structured response:
- verdict: phishing / suspicious / legitimate
- confidence: 0–100
- reason: explanation

## Step 5: Conditional Routing

Path 1:
- phishing + confidence ≥ 61
→ High-risk phishing

Path 2:
- suspicious OR low-confidence phishing
→ Needs manual review

Path 3:
- legitimate
→ Safe email

## Step 6: Actions

Phishing:
- Send alert email
- Apply "Phishing" label

Suspicious:
- Send warning
- Apply "Suspicious" label

Legitimate:
- Mark as read