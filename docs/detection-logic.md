# Detection Logic

## Signals Evaluated

### 1. Sender Analysis
- Domain spoofing
- Lookalike domains
- Free email providers

### 2. Header Indicators
- SPF/DKIM/DMARC issues
- Suspicious relay paths

### 3. Social Engineering
- Urgency ("act now")
- Threats ("account will be locked")
- Credential harvesting

### 4. Link Analysis
- Mismatched anchor text
- URL shorteners
- IP-based URLs

### 5. Content Signals
- Poor grammar
- Generic greetings
- Brand impersonation

### 6. Attachments
- Executables
- Macro-enabled files
- Password-protected archives

## Scoring Model

0–30 → Weak signals  
31–60 → Suspicious  
61–80 → Strong phishing indicators  
81–100 → Highly likely phishing  

## Key Insight

Confidence scoring enables:
- Reduced false positives
- Better prioritization
- Real-world usability