# Wireshark Network Traffic Investigation (DNS + HTTP + TLS)

## Overview
This project demonstrates basic packet capture and investigation skills using Wireshark.
The goal is to capture normal browsing traffic in a controlled setting and analyze:
- DNS queries and responses
- HTTP request/response behavior (if present)
- TLS handshake metadata for HTTPS sessions

## Objectives
- Capture traffic from a local interface (Wi-Fi/Ethernet)
- Identify DNS lookups and resolved IP addresses
- Detect HTTP traffic and extract key fields
- Analyze TLS handshakes (version, SNI, ciphers)
- Produce a short investigation-style write-up

## Tools Used
- Wireshark (free)
- A web browser
- (Optional) nslookup / dig

## Ethics / Scope
Traffic captured is from my own device/network activity in a lab-safe context.
No credential harvesting, no interception of other users’ traffic.

## Step-by-Step
1. Install Wireshark
2. Select the correct interface (Wi-Fi or Ethernet)
3. Start capture
4. Generate traffic:
   - Visit a few websites in your browser
   - Optional: run `nslookup example.com`
5. Stop capture after 2–5 minutes
6. Apply filters and document findings:
   - DNS: `dns`
   - HTTP: `http`
   - TLS handshakes: `tls.handshake`
7. Save capture file as `.pcapng`

## Key Filters (Cheat Sheet)
- `dns`
- `dns.qry.name contains "google"`
- `http`
- `http.request`
- `tls.handshake`
- `tcp.port == 443`
- `ip.addr == x.x.x.x` (filter specific IP)

## Report Template (What to Document)
- Time window of capture
- Top DNS queries observed
- DNS response IPs for 1–2 domains
- Any HTTP hosts or URIs observed (if present)
- TLS handshake notes:
  - SNI (Server Name Indication)
  - TLS version
  - Selected cipher suite

## Project Files (Suggested)
- `/pcaps/` : saved capture files (small + sanitized)
- `/report/` : investigation write-up in Markdown

## Disclaimer
This project is for learning and ethical analysis only. Only capture traffic you are authorized to capture.
