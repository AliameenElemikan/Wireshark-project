#  Network Traffic Analysis & Protocol Investigation Using Wireshark

## 📌 Executive Summary

This project demonstrates live network traffic capture and protocol analysis using Wireshark on a Windows system. The objective was to analyze HTTP, DNS, TCP handshake behavior, session termination, and DHCP address assignment in order to understand how network traffic is transmitted, reconstructed, and potentially exposed.

The investigation highlights the visibility of unencrypted HTTP traffic, DNS resolution patterns, TCP reliability mechanisms, and DHCP behavior. From a security perspective, this project demonstrates how network metadata and session information can be monitored, reconstructed, and analyzed by defenders — or intercepted by attackers on unsecured networks.

---

## 🛠 Tools Used

- Wireshark
- Windows Command Prompt
- Capture Filters
- Display Filters
- TCP Stream Reconstruction

**Environment:**
- OS: Windows
- Network: Local LAN
- Protocol Focus: HTTP (Port 80), DNS (Port 53), TCP, DHCP

---

# 🌐 1. HTTP Traffic Analysis (Port 80)

## Objective
Capture and analyze an HTTP GET request and inspect header data transmitted in plaintext.

## Actions Performed

- Captured traffic using `tcp port 80`
- Identified:
  - Source IP address
  - Destination IP address
  - Source (ephemeral) port
  - Destination port (80)
  - HTTP version (HTTP/1.1)
- Expanded TCP and IP header fields
- Inspected transmitted cookies
- Observed `Connection: Keep-Alive`

## Key Findings

- The HTTP GET request was transmitted in plaintext.
- All headers, including cookies, were fully visible in Wireshark.
- Source port was an ephemeral client port.
- Destination port was 80 (standard HTTP).

## 🔐 Security Implications

Because HTTP is unencrypted, all request headers, cookies, IP addresses, and session metadata are visible to anyone capable of capturing traffic on the same network.

This demonstrates:

- Risk of session hijacking
- Exposure of cookies in plaintext
- Interception risks on public Wi-Fi
- Why HTTPS (TLS encryption) is critical

---

# 🌍 2. DNS Traffic Investigation (Port 53)

## Objective
Analyze DNS resolution behavior and observe how domain names are translated into IP addresses.

## Actions Performed

- Created capture filter for `port 53`
- Visited external websites
- Observed DNS query and response packets
- Identified resolved IP addresses

## Key Findings

- Multiple DNS packets were generated for a single website visit.
- DNS requests were broadcast before HTTP traffic began.
- Domain name resolution occurred before TCP connection establishment.

## 🔐 Security Implications

DNS traffic reveals browsing activity even before a website loads.

This demonstrates:

- How DNS logs can be used in threat hunting
- How attackers can monitor DNS queries
- Why DNS filtering is important
- The role of DNS in detecting malicious domains

DNS monitoring is commonly used in SOC environments to identify command-and-control beaconing and suspicious domain activity.

---

# 🔁 3. TCP Three-Way Handshake & Session Analysis

## Objective
Break down the TCP connection lifecycle and analyze reliability mechanisms.

## Connection Establishment

Identified:
- SYN
- SYN/ACK
- ACK

Confirmed proper three-way handshake between client and server.

## Data Transfer

- Located HTTP GET request
- Identified corresponding ACK packets
- Used SEQ/ACK analysis to confirm acknowledgement behavior

## Connection Termination

Identified:
- FIN/ACK
- FIN/ACK response
- Final ACK

Observed graceful TCP session teardown.

## 🔐 Security Implications

Understanding TCP handshake behavior is critical for detecting:

- SYN flood attacks
- Abnormal session terminations
- Session hijacking attempts
- Incomplete handshakes
- Suspicious connection resets (RST packets)

The SEQ/ACK analysis demonstrated how TCP ensures reliable data delivery and session integrity.

---

# 🔎 4. TCP Stream Reconstruction

## Objective
Reconstruct the full client-server conversation.

## Actions Performed

- Used **Follow TCP Stream**
- Reassembled entire HTTP conversation

## Findings

The full request and response exchange was reconstructed in readable format.

## 🔐 Security Implications

TCP stream reconstruction demonstrates how analysts can:

- Rebuild session activity
- Inspect transmitted data
- Recover credentials sent over HTTP
- Investigate malicious payload delivery

This technique is commonly used in digital forensics and incident response investigations.

---

# 🏠 5. DHCP Analysis

## Objective
Observe IP address assignment using DHCP.

## Actions Performed

- Released IP address using `ipconfig /release`
- Renewed IP address using `ipconfig /renew`
- Captured DHCP traffic
- Filtered for DHCP packets

## Observed DHCP Process

- DHCP Discover
- DHCP Offer
- DHCP Request
- DHCP ACK

Observed use of IP address `0.0.0.0` during discovery phase.

## 🔐 Security Implications

The presence of 0.0.0.0 reflects a client without an assigned IP during DHCP discovery.

Monitoring DHCP traffic is important for detecting:

- Rogue DHCP servers
- Network misconfigurations
- Unauthorized device connections
- ARP spoofing preparation activity

---

# ⚠️ Overall Security Observations

This investigation demonstrated:

- Plaintext HTTP exposes sensitive data.
- DNS traffic reveals browsing activity.
- TCP handshake analysis confirms connection reliability.
- Cookies transmitted over HTTP are vulnerable.
- DHCP behavior reveals network configuration details.

From a defensive perspective, this type of analysis is essential for:

- Threat detection
- Network monitoring
- Incident response
- Packet-level forensic investigation

---

# 🎯 Skills Demonstrated

- Packet capture and filtering
- Protocol inspection (TCP/IP)
- Port analysis
- DNS monitoring
- TCP handshake analysis
- Session reconstruction
- DHCP traffic analysis
- Security risk identification

---

# 🚀 Conclusion

This project demonstrates hands-on network traffic analysis using Wireshark in a Windows environment. The investigation highlights how unencrypted traffic can be inspected, reconstructed, and analyzed — reinforcing the importance of encryption, DNS monitoring, and protocol-level visibility in cybersecurity operations.
