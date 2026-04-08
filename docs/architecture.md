# System Architecture

## Overview

The system is designed as a multi-stage autonomous pipeline that processes incoming emails and applies AI-driven threat analysis before executing risk-based actions.

## Components

1. Gmail Trigger Layer
   - Captures incoming emails in real time
   - Configured to process only the first email in a thread

2. Pre-Processing Layer
   - Extracts relevant metadata:
     - Sender
     - Subject
     - Email body (partial)
     - URLs

3. Allowlist Filter
   - Skips trusted domains (e.g., Google, Microsoft, Amazon)
   - Reduces processing cost and false positives

4. AI Analysis Engine
   - Uses LLM (Claude Sonnet) for phishing detection
   - Produces structured output:
     - verdict
     - confidence
     - reasoning

5. Decision Engine
   - Routes execution based on classification + confidence

6. Action Layer
   - Sends alerts
   - Applies Gmail labels
   - Marks safe emails as read

## Design Goal

Balance:
- Accuracy
- Cost efficiency
- Low false positives
- Real-time usability