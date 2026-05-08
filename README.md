# N8N-SOC-Alert-Automation-Project
A cybersecurity automation project focused on SOC alert analysis, threat detection, and incident response workflows. This project is designed to automate the processing of security alerts generated from SIEM/XDR platforms such as Wazuh, Splunk, or Elastic Stack.

#🛡️ Phishing Detection SOAR Workflow (n8n)
An automated Security Orchestration, Automation and Response (SOAR) pipeline built with n8n that detects and triages phishing emails using threat intelligence and LLM-based analysis.

📌 Overview
This workflow automatically:

Receives email metadata via a Webhook
Enriches the sender IP via VirusTotal
Analyzes the email body with an LLM for social engineering techniques
Merges both results and evaluates severity via an If condition
Creates an incident ticket if the email is flagged as malicious
Routes borderline/suspicious cases to a Wait (human-in-the-loop) step
