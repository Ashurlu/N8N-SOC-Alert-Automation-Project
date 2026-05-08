# 🔁 Workflow Architecture

 ```
Webhook → [VirusTotal (IP Reputation)]
        → [LLM (Email Analysis)]
              ↓
           Merge
              ↓
             If (malicious score > 3 OR confidence > 7)
           /      \
        true      false
          ↓          ↓
   Create Ticket   Wait (human review)
```
<img width="786" height="425" alt="WhatsApp Image 2026-05-08 at 13 10 49" src="https://github.com/user-attachments/assets/ff976f8b-cc45-4f01-aef9-bee1116950ce" />

 ```
| Node | Type | Purpose |
|------|------|---------|
| **Webhook** | Trigger | Receives email JSON via POST |
| **Virus Total** | HTTP Request | Looks up sender IP reputation |
| **LLM** | HTTP Request | Analyzes email body for phishing indicators |
| **Merge** | Merge | Combines VT + LLM results (Append mode) |
| **If** | Condition | Routes based on malicious score or confidence |
| **HTTP Request** | Action | Creates incident ticket (POST to `/create-ticket`) |
| **Wait** | Control | Pauses for human review on borderline cases |

```
