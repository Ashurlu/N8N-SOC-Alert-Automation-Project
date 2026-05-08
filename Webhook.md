Phishing Detection Entry Point

This node receives incoming email metadata from external 
systems such as SIEM, email gateways, or custom scripts.

Input: JSON payload containing email fields
  - email_id, timestamp, sender, sender_ip
  - recipient, subject, body
  - urls, attachment_hash
  - headers (spf, dkim, return_path, x_mailer)

Output: Full email data passed in parallel
        to VirusTotal and LLM nodes

Test endpoint: POST /webhook-test/phishing
Prod endpoint: POST /webhook/phishing

<img width="1371" height="568" alt="Screenshot 2026-05-08 122605" src="https://github.com/user-attachments/assets/fd85de02-ea2c-4d9d-886e-edbe234251ed" />

##  How to send this type of alert via linux to webhook

```bash
curl -X POST http://localhost:5678/webhook-test/phishing \
  -H "Content-Type: application/json" \
  -d @/opt/lab16/phishing_malicious.json
```

  - curl -> Command-line tool for making HTTP requests 
  - http://localhost:5678 -> n8n is running locally on port 5678 (In Docker)
  - -X POST -> Sends an HTTP POST request
  - /webhook-test/phishing -> The test webhook endpoint (not production)
  - -H "Content-Type: application/json" -> Tells the server the body is JSON format
  - -d @/opt/lab16/phishing_malicious.json -> Reads and sends the JSON file as the request body
```bash
{"message":"Workflow was started"}
```
This confirms that:
  - n8n received the request successfully
  - The webhook node triggered the workflow
  - The phishing email data is now flowing through the pipeline (VirusTotal → LLM → Merge → If → ...)

##  Input Format
 
The webhook expects a JSON payload like:
 
```json
{
  "email_id": "MSG-20260315-001",
  "timestamp": "2026-03-15T09:14:22.000Z",
  "sender": "it-security@c0rporate-help.xyz",
  "sender_ip": "203.0.113.45",
  "recipient": "john.smith@company.com",
  "subject": "URGENT: Your account will be locked in 24 hours",
  "body": "Dear Employee,\n\nOur security system has detected unusual activity...",
  "urls": ["http://secure-login.totallylegit.xyz/verify?user=john.smith"],
  "attachment_hash": "e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855",
  "headers": {
    "return_path": "bounce@c0rporate-help.xyz",
    "x_mailer": "PHPMailer 6.1.4",
    "spf": "fail",
    "dkim": "none"
  }
}
```
 
Sample files are provided in the [`/samples`](./samples/) directory.
 
---

## The executuion must be like this: 

<img width="2510" height="1223" alt="Screenshot 2026-05-08 111331" src="https://github.com/user-attachments/assets/c09b5062-a8b0-47e8-af4f-14ff1d1991bb" />

