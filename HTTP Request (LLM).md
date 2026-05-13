## AI-Powered Email Analysis

Sends the email body to an LLM endpoint for 
deep phishing content analysis.

Input:  email_body from Webhook
URL:    POST /analyze

Output:
  - summary: human-readable explanation
  - social_engineering_techniques: list of tactics used
  - iocs_extracted: suspicious URLs and IPs
  - confidence_score: 0-10 (10 = definitely phishing)
  - verdict: phishing / suspicious / legitimate

In production: replace mock URL with 
OpenAI, Anthropic, or a local LLM endpoint.

<img width="1600" height="802" alt="WhatsApp Image 2026-05-08 at 20 36 14" src="https://github.com/user-attachments/assets/6262521e-cef6-4a12-823d-1f208ed22e41" />

## Screenshot Description
n8n — LLM Node (HTTP Request1): AI-Powered Email Analysis
This screenshot shows the LLM node inside the n8n workflow editor. It sends the email body to an AI analysis endpoint that detects social engineering techniques, extracts indicators of compromise (IOCs), and returns a phishing verdict with a confidence score.

 - Method -> POST -> Sends data to the LLM for processing
 - URL -> http://3.82.6.129:9001/analyzeMock -> LLM analysis endpoint on port 9001
 - Authentication -> None -> Mock API — in production an API key is required
 - Send Query Parameters -> OFF -> No URL query params needed
 - Send Headers -> OFF -> No custom headers needed
 - Send Body -> ON -> Email body is sent as JSON payload
 - Body Content Type -> JSON -> Data formatted as JSON
 - Specify Body -> Using JSON -> Manual JSON expression used

## Request Body (JSON Expression):

```json
{
  "email_body": {{ JSON.stringify($json.body.body) }}
}
```
 - "email_body" -> The field name the LLM endpoint expects
 - JSON.stringify(...) -> Safely serializes the email text, escaping special characters
 - $json.body.body -> n8n expression pulling the email body from the webhook input

## Resolved preview:
```json
{ "email_body": "Dear Employee,\n\nOur security system..." }
```

## Right Panel — OUTPUT
The LLM analysis endpoint returned a structured threat assessment:

```json
{
  "summary": "This email impersonates the IT department and creates urgency by claiming the recipient's account will be locked. It contains a suspicious link to a credential harvesting page.",
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

   
## LLM Output Format
The LLM analysis node returns structured JSON:
```json
{
  "summary": "...",
  "social_engineering_techniques": ["authority impersonation", "urgency/fear", "credential harvesting link"],
  "iocs_extracted": ["http://suspicious-url.xyz/verify", "203.0.113.45"],
  "confidence_score": 9,
  "verdict": "phishing"
}
```

## Key Takeaways

The confidence score of 9 will trigger the If node's True branch (condition: confidence_score > 7)
The LLM identified 3 distinct social engineering tactics used simultaneously — a strong multi-layered attack
Both the phishing URL and sender IP were automatically extracted as IOCs without manual analysis
This node runs in parallel with the VirusTotal node — both results are then merged downstream for combined evaluation
In production, replace the mock URL with OpenAI, Anthropic Claude, or a local LLM endpoint with a proper system prompt for email triage

