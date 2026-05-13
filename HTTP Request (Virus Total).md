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
GET -> Read-only lookup request
.../ip_addresses/{{ $json.body.sender_ip }} -> n8n expression dynamically inserts 203.0.113.45 from the email payload

