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

## Decision Flow Summary

```json
VirusTotal malicious = 8  →  8 > 3  ✅ TRUE
                                        OR
LLM confidence_score = 9  →  9 > 7  ✅ TRUE
                                        ↓
                              TRUE BRANCH TRIGGERED
                                        ↓
                           → Create Incident Ticket
```
## Key Takeaways

The If node acts as the automated triage engine — no human needed for clear-cut cases
Using OR logic means the workflow catches threats even if only one source flags it — reducing false negatives
Both signals independently confirmed this as malicious, giving maximum confidence in the automated response
The False branch handles edge cases — borderline emails are not silently dropped but held for analyst review via the Wait node
In production, thresholds (3 and 7) can be tuned based on the organization's risk tolerance and false positive rate
