# Investigation 02 - ICMP Traffic Analysis

## Objective

To understand how ICMPv6 Echo Request and Echo Reply packets are used to verify network connectivity by generating ICMP traffic using the Windows ping utility and analyzing the captured packets using Wireshark.

## Findings
-> Observed an ICMPv6 Echo Request.

-> Observed a corresponding ICMPv6 Echo Reply.

-> Successfully verified network connectivity between the client and the destination host.

-> Wireshark automatically correlated the Echo Request with its corresponding Echo Reply.

## Investigation Environment
Operating System:
-> Windows 11

Analysis Tool:
-> Wireshark

Traffic Generator:
-> Windows Command Prompt (ping)

Protocol:
-> ICMPv6

## Evidence Collected

Capture File : icmp_capture_01.pcapng

Packet Numbers:
-> Echo Request : Packet 391

-> Echo Reply : Packet 392

Protocol : ICMPv6

Message Types:
-> Echo Request (128)

-> Echo Reply (129)

## Communication Timeline

## Step 1 – Echo Request

Packet Number : 391

Protocol : ICMPv6

Protocol Stack:

Layer 2 : Ethernet II

Layer 3 : IPv6

Network Protocol : ICMPv6

Source (Client) : 2401:4900:9704:874f:8a9:4c95:4f49:ab24

Destination (Google Server) : 2404:6800:4007:816::200e

Hop Limit : 128

Event:
The client generated an ICMPv6 Echo Request to verify network connectivity with the destination host.

Observation:

The packet was transmitted successfully to the destination. Wireshark identified that the corresponding Echo Reply would be received in Packet 392.

## Step 2 – Echo Reply

Packet Number : 392

Protocol:

ICMPv6

Protocol Stack:

Layer 2 : Ethernet II

Layer 3 : IPv6

Network Protocol : ICMPv6

Source (Google Server) : 2404:6800:4007:816::200e

Destination (Client) : 2401:4900:9704:874f:8a9:4c95:4f49:ab24

Hop Limit : 117

Event:
The destination host generated an ICMPv6 Echo Reply, confirming successful communication with the client.

Observation:
The source and destination IPv6 addresses were reversed compared to the Echo Request. The Identifier and Sequence Number remained unchanged, allowing Wireshark to correlate the Echo Reply with the original request.

## Security Assessment

Legitimacy Assessment:
The ICMPv6 traffic was intentionally generated using the Windows ping utility to verify connectivity with a legitimate Google server.

Behavioral Assessment:
The communication followed the expected ICMPv6 request-response pattern without any abnormal behavior.

Indicators of Suspicious Activity:
No malformed packets, unexpected ICMP message types, abnormal packet sizes, or suspicious communication patterns were observed.

Overall Assessment:
The captured traffic represents normal ICMPv6 network connectivity testing and does not indicate any suspicious or malicious activity.

## Conclusion
This investigation analyzed the ICMPv6 communication process by capturing and examining normal ping traffic generated from a Windows system. The captured packets demonstrated a successful Echo Request and Echo Reply exchange between the client and the destination host.
The analysis confirmed successful network connectivity, proper packet correlation, and normal ICMPv6 communication behavior. Based on the collected evidence, no suspicious activity or indicators of compromise were identified during this investigation.