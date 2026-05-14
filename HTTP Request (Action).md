## Incident Ticket Creation

Creates a security incident ticket when an email 
is confirmed malicious by the triage condition.  
Method: POST
URL:    /create-ticket (port 9002)

Payload:
  - subject: LLM verdict
  - sender:  original email sender address
  - verdict: VirusTotal malicious count

In production: replace mock API with 
Jira, ServiceNow, or PagerDuty integration.

## 🎫 Ticket Format
When a malicious email is detected, a ticket is created via POST to /create-ticket:
```json
{
  "subject": "phishing",
  "sender": "it-security@c0rporate-help.xyz",
  "verdict": 8
}
```
<img width="2512" height="1249" alt="Screenshot 2026-05-08 120840" src="https://github.com/user-attachments/assets/20b447d6-895a-4d7e-9c64-20f769496013" />
This screenshot shows the Create Ticket node (final action node on the True branch). When the If node confirms a malicious email, this node automatically creates a security incident ticket by sending a POST request to the ticketing API.  
## Middle Panel — CONFIGURATION


```json
Method:  POST
URL:     http://3.82.6.129:9002/create-ticket
```


  - Method -> POST -> Creates a new ticket record  
  - URL -> .../create-ticket on port 9002 -> Mock ticketing API endpoint  
  - Authentication -> None -> Mock API — in production requires auth token  
  - Send Query Parameters -> OFF  -> No URL params needed  
  - Send Headers -> OFF -> No custom headers needed  
  - Send Body -> ON -> Ticket data sent as JSON  
  - Body Content Type -> JSON -> Structured JSON payload  
  - Specify Body -> Using Fields Below -> Fields defined individually  

## Body Parameters (3 fields mapped):

