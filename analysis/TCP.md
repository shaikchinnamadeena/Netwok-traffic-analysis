Investigation 003 – TCP Traffic Analysis
## Objective

The objective of this investigation is to analyze the lifecycle of a TCP connection using Wireshark. The analysis focuses on connection establishment, application data transport, TCP reliability mechanisms, and graceful connection termination to understand how TCP ensures reliable communication across a network.

## Lab Environment
Component	Details
Host Machine	Windows 11
Packet Analyzer	Wireshark
Protocol	TCP
Application	HTTPS (www.wireshark.org)
Network Stack	Ethernet → IPv6 → TCP → TLS
## Investigation Summary:
A TCP conversation (Stream 0) was isolated using Wireshark. The captured session demonstrated:
TCP Three-Way Handshake
TLS handshake initiation
Secure application data transfer
TCP retransmission
Graceful connection termination

### Packet Analysis
# Packet 20 – TCP SYN
Purpose:
Packet 20 represents the first step of the TCP three-way handshake. The client initiates communication with the server by sending a SYN packet requesting a TCP connection.
Key Observations
Source Port: 58572
Destination Port: 443 (HTTPS)
TCP Flag: SYN
Sequence Number: 0 (Relative)
Window Size: 65535
MSS: 1440 Bytes
Window Scale: 8 (×256)
SACK Permitted: Enabled
TCP Payload Length: 0 Bytes
Analysis:
The packet contains no application payload because it is used solely to establish a TCP session. During this stage, the client advertises its TCP capabilities, including Maximum Segment Size (MSS), Window Scaling, and Selective Acknowledgment (SACK).

Observation:
The client successfully initiated a TCP connection using standard TCP negotiation parameters. No abnormalities were observed.

# Packet 28 – TCP SYN, ACK
Purpose:
Packet 28 represents the second step of the TCP three-way handshake. The server acknowledges the client's SYN request while simultaneously sending its own SYN.

Key Observations
Source Port: 443
Destination Port: 58572
TCP Flags: SYN, ACK
Sequence Number: 0 (Relative)
Acknowledgment Number: 1
MSS: 1340 Bytes
Window Scale: 13 (×8192)
SACK Permitted: Enabled
Analysis:
The server accepted the connection request and advertised its own TCP capabilities. The acknowledgment number confirms successful receipt of the client's SYN packet.

Observation:
The server responded normally with no indication of connection failure or packet loss.

# Packet 30 – TCP ACK
Purpose:
Packet 30 completes the TCP three-way handshake.

Key Observations
Source Port: 58572
Destination Port: 443
TCP Flag: ACK
Sequence Number: 1
Acknowledgment Number: 1
Header Length: 20 Bytes
TCP Payload Length: 0 Bytes
Analysis:
The client acknowledges the server's SYN packet, completing the TCP connection establishment process. Since TCP options were negotiated in the previous packets, the header returns to its standard size of 20 bytes.

Observation:
The TCP connection was successfully established and is now ready to transport application-layer traffic.

# Packet 31 – TLS Client Hello
Purpose
Following the successful TCP handshake, the client initiates a TLS handshake by sending a Client Hello message through the established TCP connection.

Key Observations
TCP Flags: PSH, ACK
TCP Payload Length: 1727 Bytes
Protocol: TLS 1.3 Client Hello
Analysis:
TCP is now transporting application-layer data. The payload contains the TLS Client Hello message, marking the beginning of encrypted communication.

Observation:
TCP successfully transitioned from connection establishment to reliable transport of application-layer data.

# Packet 52 – TLS Application Data
Purpose:
Packet 52 demonstrates the transmission of encrypted application data over the established TCP connection.

Key Observations
Source Port: 443
Destination Port: 58572
TCP Flags: PSH, ACK
TCP Payload Length: 567 Bytes
Protocol: TLS Application Data

Analysis:
The TLS handshake has completed sufficiently for encrypted communication to begin. Wireshark reassembled multiple TCP segments (#49, #50, and #52) into a single TLS record, demonstrating TCP segmentation and reassembly.

Observation:
TCP successfully transported encrypted application data without communication errors.

# Packet 126 – TCP Retransmission
Purpose:
Packet 126 demonstrates TCP's reliability mechanism by retransmitting previously sent data.

Key Observations
Source Port: 443
Destination Port: 58572
TCP Flags: PSH, ACK
TCP Payload Length: 31 Bytes
Wireshark Expert Information: Suspected Retransmission

Analysis:
The server retransmitted a previously sent TCP segment after an expected acknowledgment was not received within the retransmission timeout. The retransmitted segment was acknowledged immediately afterward, indicating normal TCP recovery behavior.

Observation:
A single suspected retransmission was observed. The connection continued normally without repeated retransmissions or connection failures, indicating expected TCP reliability rather than malicious activity.

# Packet 128 – TCP Connection Termination
Purpose
Packet 128 marks the beginning of the TCP connection termination process.

Key Observations:
Source Port: 58572
Destination Port: 443
TCP Flags: FIN, ACK
TCP Payload Length: 0 Bytes
Analysis

The client informs the server that it has completed data transmission and requests a graceful connection closure. The remaining FIN/ACK exchange completes the standard TCP four-way termination process.

Observation:
The connection terminated normally without TCP Reset (RST) packets or abnormal session termination.

## TCP Session Flow
Packet 20   → SYN
        ↓
Packet 28   → SYN, ACK
        ↓
Packet 30   → ACK
        ↓
Packet 31   → TLS Client Hello
        ↓
Packet 52   → Encrypted Application Data
        ↓
Packet 126  → Suspected TCP Retransmission
        ↓
Packet 128  → FIN, ACK
        ↓
Server FIN, ACK
        ↓
Final ACK
        ↓
Connection Closed

## Key Findings:
A successful TCP three-way handshake established the connection.
TCP options (MSS, Window Scale, and SACK) were negotiated during connection establishment.
The established TCP connection transported TLS handshake messages and encrypted application data.
A single suspected TCP retransmission demonstrated TCP's built-in reliability mechanism.
The session terminated gracefully using the standard TCP four-way termination process.
No abnormal TCP behavior, repeated retransmissions, or connection resets were observed.

## Conclusion: 
This investigation demonstrated the complete lifecycle of a TCP connection, from connection establishment to graceful termination. The captured traffic showed standard TCP behavior, including successful three-way handshake negotiation, reliable transport of encrypted application data, automatic recovery through retransmission, and orderly session termination.
Overall, the analyzed TCP conversation exhibited normal network behavior with no indicators of malicious activity or protocol anomalies.