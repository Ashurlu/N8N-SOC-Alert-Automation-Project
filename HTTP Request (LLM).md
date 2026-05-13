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

