## Severity Triage Condition

Evaluates combined threat signals and routes 
the workflow to the appropriate response branch.

Condition (True = Malicious):
  - VirusTotal malicious count > 3
    OR
  - LLM confidence_score > 7

True branch  → Create incident ticket (HTTP Request)
False branch → Route to human review (Wait node)
