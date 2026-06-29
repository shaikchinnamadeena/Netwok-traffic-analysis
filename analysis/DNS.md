# Investigation 01 - DNS Traffic Analysis
## Objective 
To understand DNS query and response behavior by generating normal DNS traffic and analyzing the captured packets using Wireshark.
## Key Findings
->Observed a DNS Standard Query.
-> Observed a corresponding DNS Standard Query Response.
-> The client requested an AAAA (IPv6) record.
-> The DNS server returned a CNAME record followed by an AAAA record.
## Investigation Setup
Operating System:
-> Windows 11
Analysis Tool:
-> Wireshark
Traffic Generator
-> Google chrome
## Investigation Timeline
Step 1 – DNS Query
Packet Number: 196
Protocol: DNS
Source (Client):
2401:4900:96f0:cbff:85a:e35f:ebbe:45ef
Destination (DNS Server):
2401:4900:96f0:cbff::99
Query Type: AAAA (IPv6 Address)
Requested Domain: lh3.google.com
Event:
The client initiated a DNS lookup by sending a standard AAAA query requesting the IPv6 address of lh3.google.com.

Step 2 – DNS Response
Packet Number: 248
Protocol: DNS
Source (DNS Server):
2401:4900:96f0:cbff::99
Destination (Client):
2401:4900:96f0:cbff:85a:e35f:ebbe:45ef
Answer Resource Records: 2
Returned Records:
CNAME → lh3.google.com → lh2.l.google.com
AAAA → lh2.l.google.com → 2404:6800:4007:800::200e
Event:
The DNS server successfully resolved the client's request by returning two resource records: a CNAME record identifying the canonical hostname and an AAAA record containing the IPv6 address.

Step 3 – QUIC Communication
Packet Number: 251
Protocol: QUIC
Source (Client):
2401:4900:96f0:cbff:85a:e35f:ebbe:45ef
Destination (Google Server):
2404:6800:4007:800::200e
Event:
After receiving the IPv6 address from the DNS response, the client initiated QUIC communication with the resolved Google server.
Observation:
The browser used QUIC (HTTP/3 over UDP) instead of the traditional TCP/TLS communication.
## Security Assessment
Legitimacy Assessment: The queried domain (lh3.google.com) belongs to Google's infrastructure and is considered legitimate.
Behavioral Assessment: The client successfully resolved the domain and initiated QUIC communication, which is expected for modern browsers using HTTP/3.
Indicators of Suspicious Activity: No suspicious domains, unexpected DNS records, or abnormal communication patterns were observed.
Overall Assessment: The captured traffic represents normal DNS resolution associated with routine web browsing.
## Conclusion
This investigation analyzed the DNS resolution process by capturing and examining normal network traffic generated during web browsing. The captured packets showed a successful DNS query and response for lh3.google.com, where the DNS server returned both CNAME and AAAA resource records before the client initiated QUIC communication with the resolved IPv6 address.
Based on the captured evidence, the observed network activity was consistent with legitimate DNS resolution and normal browser behavior. No suspicious DNS activity or indicators of compromise were identified during this investigation.