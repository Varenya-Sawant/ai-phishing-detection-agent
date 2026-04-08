      ┌─────────────────────┐
        │   📨 New Email      │
        │   (Gmail Trigger)   │
        └────────┬────────────┘
                 │
                 ▼
        ┌─────────────────────┐
        │ Trusted Sender?     │
        │ @google, @github,   │
        │ @microsoft, @apple, │
        │ @amazon, @linkedin, │
        │ @slack, @notion,    │
        │ @stripe             │
        └────┬───────────┬────┘
          YES│           │NO
             ▼           ▼
        ┌────────┐  ┌──────────────────┐
        │ 🛑 END │  │ 🤖 AI Analysis   │
        └────────┘  │ (Claude Sonnet)  │
                    │                  │
                    │ Output:          │
                    │ • verdict        │
                    │ • confidence (%) │
                    │ • reason         │
                    └────────┬─────────┘
                             │
              ┌──────────────┼──────────────┐
              ▼              ▼              ▼
  ┌───────────────┐ ┌──────────────┐ ┌───────────┐
  │ 🚨 PHISHING   │ │ ⚠️ SUSPICIOUS │ │ ✅ LEGIT   │
  │ verdict =     │ │ verdict =    │ │ verdict = │
  │ phishing AND  │ │ suspicious   │ │ legitimate│
  │ confidence≥61 │ │ OR phishing  │ │           │
  │               │ │ + conf < 61  │ │           │
  └──────┬────────┘ └──────┬───────┘ └─────┬─────┘
         │                 │               │
         ▼                 ▼               ▼
  ┌──────────────┐ ┌──────────────┐ ┌───────────┐
  │ 📧 Alert     │ │ 📧 Warning   │ │ ✔ Mark as │
  │ Email sent   │ │ Email sent   │ │ read      │
  └──────┬───────┘ └──────┬───────┘ └───────────┘
         │                 │
         ▼                 ▼
  ┌──────────────┐ ┌──────────────┐
  │ 🏷 Label:    │ │ 🏷 Label:    │
  │ "Phishing"   │ │ "Suspicious" │
  └──────────────┘ └──────────────┘