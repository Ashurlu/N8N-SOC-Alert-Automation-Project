## Severity Triage Condition

Evaluates combined threat signals and routes 
the workflow to the appropriate response branch.

Condition (True = Malicious):
  - VirusTotal malicious count > 3
    OR
  - LLM confidence_score > 7

True branch  → Create incident ticket (HTTP Request)
False branch → Route to human review (Wait node)

<img width="1600" height="798" alt="WhatsApp Image 2026-05-08 at 20 36 14 (1)" src="https://github.com/user-attachments/assets/576accb4-4033-4fbb-b20c-df7081e044bd" />

## 🔀 If Condition Logic
The workflow triggers the True (malicious) branch when:

VirusTotal malicious score > 3
OR
LLM confidence_score > 7

Otherwise, it routes to the False branch (Wait / human review).

## This shows the If node inside the n8n workflow editor.
This is the brain of the automation — it evaluates the combined threat intelligence from both VirusTotal and the LLM, then automatically routes the workflow to either create an incident ticket (True branch) or send to human review (False branch).

## Middle Panel — CONDITIONS
Condition 1:
```json
{{ $json.data.attributes.last_analysis_stats.malicious }}
is greater than
3
```
→ Checks if VirusTotal malicious count > 3  
→ Current value: 8 ✅ TRUE  
## Operator between conditions:

```json
OR
```
→ Only one condition needs to be true to route to the True branch  
  
Condition 2:
```json
{{ $json.confidence_score }}
is greater than
7
```
→ Checks if LLM confidence score > 7  
→ Current value: 9 ✅ TRUE  
 - Convert types where required -> OFF -> Values compared as-is
 - Ignore Case -> OFF -> Case-sensitive string comparison
   Both conditions are TRUE in this execution — the email passes the threshold on both checks simultaneously, making this a high-confidence automated decision.

## Right Panel — OUTPUT
The True Branch is active with 2 items passing through:

```json
{
  "data": {
    "id": "203.0.113.45",
    "type": "ip-address",
    "attributes": {
      "last_analysis_stats": {
        "malicious": 8,
        "suspicious": 3,
        "harmless": 50,
        "undetected": 23
      },
      "reputation": -28,
      "country": "CN",
      "tags": ["phishing", "spam"]
    }
  },
  "summary": "This email impersonates the IT department...",
  "social_engineering_techniques": [
    "authority impersonation",
    "urgency/fear",
    "credential harvesting link"
  ],
  "iocs_extracted": [
    "http://secure-login.totallylegit.xyz/verify",
    "203.0.113.45"
  ],
  "confidence_score": 9,
  "verdict": "phishing"
}
```

