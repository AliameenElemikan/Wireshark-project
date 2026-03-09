# Network Traffic Investigation with Wireshark (SOC-Focused Analysis)

## Overview

This project demonstrates packet-level network analysis using **Wireshark** to investigate HTTP, DNS, TCP, and DHCP activity. The objective was to simulate the type of traffic inspection commonly performed by **Security Operations Center (SOC) analysts** when investigating network activity.

Through controlled packet capture and protocol inspection, this investigation highlights how network metadata and session information can be observed, reconstructed, and analyzed. The analysis also demonstrates how unencrypted traffic exposes sensitive information such as HTTP headers and cookies.

This project focuses on identifying and interpreting network behavior rather than simply capturing packets, aligning the analysis with common **SOC investigation techniques** used in threat detection and incident response.

---

# 🛠 Tools Used

- Wireshark
- Windows Command Prompt
- Packet capture filters
- Display filters
- TCP Stream reconstruction

**Environment**

- Operating System: Windows  
- Network Interface: Wi-Fi  
- Protocol Focus:
  - HTTP (Port 80)
  - DNS (Port 53)
  - TCP
  - DHCP

---

# 1️⃣ Capture Methodology

To isolate web traffic, a capture filter was applied to collect packets using **TCP port 80**, which corresponds to standard HTTP traffic. This approach allows analysts to focus on specific protocols during an investigation and reduces noise from unrelated network traffic.

![Capture Filter Applied](screenshots/01_capture_filter_port80.png)

Applying capture filters is a common technique used in network monitoring environments to efficiently collect relevant traffic during an investigation.

---

# 2️⃣ Identifying HTTP Requests

After beginning the capture and visiting a website, HTTP packets were identified within the packet list. The **GET request** visible in the Info column represents a client request to retrieve content from a web server.

![HTTP GET Packet List](screenshots/02_http_get_packet_list.png)

Important metadata visible in this view includes:

- Source IP address  
- Destination IP address  
- Source port (ephemeral client port)  
- Destination port (80)  
- HTTP request method  

SOC analysts frequently begin investigations by identifying suspicious or noteworthy requests within captured traffic.

---

# 3️⃣ Inspecting HTTP Headers

Expanding the **Hypertext Transfer Protocol** section reveals detailed information contained in the HTTP request headers.

![Expanded HTTP Request](screenshots/03_http_get_expanded.png)

This section includes metadata such as:

- Request method
- Host header
- User-Agent
- Accepted content types
- Connection behavior

Because this traffic is transmitted over **HTTP rather than HTTPS**, the entire request is visible in plaintext.

---

# 4️⃣ Plaintext Cookie Exposure

One critical observation during the inspection was the presence of **HTTP cookies transmitted in plaintext**.

![Cookie in Plaintext](screenshots/04_cookie_plaintext.png)

Cookies may contain session identifiers or tracking data used by websites. When transmitted over unencrypted HTTP connections, this information can potentially be intercepted by attackers performing packet sniffing on the same network.

This demonstrates the importance of **HTTPS encryption**, which prevents sensitive session data from being exposed during transmission.

---

# 5️⃣ TCP Three-Way Handshake

TCP connections begin with a **three-way handshake**, which establishes a reliable communication channel between client and server.

The handshake consists of:

1. SYN (client initiates connection)  
2. SYN-ACK (server acknowledges)  
3. ACK (client confirms)

![TCP Handshake](screenshots/05_tcp_handshake.png)

Understanding this process allows analysts to identify abnormal connection behavior such as:

- SYN flood attacks  
- Incomplete connections  
- Suspicious connection resets  

---

# 6️⃣ SEQ/ACK Reliability Analysis

Wireshark's **SEQ/ACK analysis** feature helps analysts understand how TCP ensures reliable data delivery.

![SEQ ACK Analysis](screenshots/06_seq_ack_analysis.png)

This analysis shows that the packet is acknowledging a previously transmitted segment. TCP uses sequence and acknowledgment numbers to verify that all data has been successfully received.

---

# 7️⃣ TCP Connection Termination

After data transfer is complete, TCP sessions are gracefully terminated using **FIN/ACK packets**.

![TCP FIN ACK](screenshots/07_tcp_fin_ack.png)

The FIN flag signals the end of the communication session between the client and server.

Analyzing connection termination behavior can help detect anomalies such as:

- Forced connection resets  
- Abrupt session terminations  
- Network disruptions  

---

# 8️⃣ TCP Stream Reconstruction

Wireshark provides a feature called **Follow TCP Stream**, which reconstructs the entire conversation between client and server.

![TCP Stream Reconstruction](screenshots/08_tcp_stream_reconstruction.png)

This allows analysts to view the complete exchange of data in readable format.

This capability is useful during:

- Incident response investigations  
- Malware traffic analysis  
- Credential interception analysis  
- Web session reconstruction  

---

# 9️⃣ System Network Identification

To verify the identity of the system generating the captured traffic, the network configuration was inspected using the Windows command: ipconfig /all


![System IP Verification](screenshots/09_system_ip_verification.png)

This confirms the system's:

- IPv4 address  
- Physical (MAC) address  

These identifiers allow analysts to correlate captured packets with the device that generated the traffic.

Note: Because IP addresses are assigned dynamically through DHCP, the IPv4 address may differ from the one observed earlier during packet capture.

---

# 🔟 DHCP Address Assignment

The Dynamic Host Configuration Protocol (DHCP) automatically assigns IP addresses to devices joining a network.

During the investigation, the DHCP exchange process was captured and analyzed.

![DHCP Exchange](screenshots/10_dhcp_exchange.png)

The DHCP process includes four steps:

1. Discover – Client requests an IP address  
2. Offer – Server offers an available address  
3. Request – Client requests the offered address  
4. ACK – Server confirms the assignment  

Monitoring DHCP traffic can help identify:

- Rogue DHCP servers  
- Unauthorized devices joining a network  
- Network configuration anomalies  

---

# ⚠️ Security Observations

This investigation revealed several important security insights:

- HTTP traffic exposes request headers and cookies in plaintext.
- Network traffic can be reconstructed to reveal full client-server communication.
- DNS and HTTP activity can reveal browsing behavior.
- TCP sequence and acknowledgment numbers maintain reliable communication.
- DHCP activity exposes how devices dynamically obtain network addresses.

These observations demonstrate how packet analysis can be used for **network monitoring, threat detection, and incident investigation**.

---

# 🎯 Skills Demonstrated

- Packet capture and filtering
- Protocol analysis (TCP/IP)
- HTTP traffic inspection
- Cookie exposure identification
- TCP handshake analysis
- TCP session reconstruction
- DHCP traffic investigation
- Network forensic techniques

---

# 🚀 Conclusion

This project demonstrates how Wireshark can be used to perform detailed network traffic analysis similar to the workflows used by security analysts in SOC environments.

By capturing and inspecting packet-level data, analysts can observe how network protocols operate, identify exposed information, and investigate communication between systems. These skills are fundamental for cybersecurity professionals involved in **network defense, threat hunting, and incident response**.

