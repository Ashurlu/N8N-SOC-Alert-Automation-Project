## IP Reputation Enrichment

Queries the VirusTotal API to check the reputation 
of the sender's IP address.

Input:  sender_ip from Webhook body
URL:    GET /api/v3/ip_addresses/{sender_ip}

Output:
  - malicious / suspicious / harmless / undetected counts
  - reputation score
  - country of origin
  - tags (e.g. phishing, spam)

In production: replace mock URL with 
https://www.virustotal.com/api/v3/ip_addresses/{ip}
with a valid API key header.

<img width="2510" height="1253" alt="Screenshot 2026-05-08 112221" src="https://github.com/user-attachments/assets/a8eb6cd1-0702-46dc-9ce6-a025f526e5b3" />
This screenshot shows the VirusTotal node (labeled "HTTP Request") inside the n8n workflow editor. It is configured to query a VirusTotal-compatible API to check the reputation of the sender's IP address extracted from the incoming phishing email.

## Middle Panel — CONFIGURATION
```
Method:  GET
URL:     http://3.82.6.129:9000/api/v3/ip_addresses/{{
$json.body.sender_ip }}
```
  - GET -> Read-only lookup request
  - .../ip_addresses/{{ $json.body.sender_ip }} -> n8n expression dynamically inserts 203.0.113.45 from the email payload
  - .../ip_addresses/203.0.113.45 -> The actual URL sent at runtime
  - None -> Mock API — in production a VT API key header is required
  - All OFF -> Pure GET request with no extra parameters
The {{ $json.body.sender_ip }} is an n8n expression — it dynamically pulls the sender IP from the webhook input so every email is checked automatically.

## Right Panel — OUTPUT
```
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
  }
}
```
## Key Takeaways

The malicious count of 8 will trigger the If node's True branch (condition: malicious > 3)
A reputation of -28 is a strong negative signal
The SPF fail + DKIM none in the email headers confirm the sender is not who they claim to be
The phishing tag from VirusTotal, combined with the LLM analysis downstream, gives the workflow high confidence to create an incident ticket automatically
