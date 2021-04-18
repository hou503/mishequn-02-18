---
title: TCP protocol
date: 2020-10-18 19:30:12
tags: TCP
category: TCP
---
The Internet is, in fact, an architecture of ideas and protocols. Of these, protocols are a set of well-known rules and standards that, if all parties agree to use them, there will be no barriers to communication between them.

Data in the Internet is transmitted in packets. If the data being sent is large, that data is split into many small packets to be transmitted. For example, the audio data you are listening to right now is split into smaller packets to be transmitted, not one large file at a time.

1. IP: delivery of packets to destination hosts
In order for a packet to be transmitted over the Internet, it has to comply with the Internet Protocol (IP) standard. Different online devices on the Internet have unique addresses, the address is just a number, which is similar to most home receiving addresses, you only need to know the specific address of a home to send a package to that address so that the logistics system can deliver the item to its destination.

The address of a computer is called an IP address, and accessing any website is really just your computer requesting information from another computer.

If you want to send a packet from host A to host B, the packet will be appended with host B's IP address information before it is transmitted so that it can be addressed correctly during transmission. In addition, the packet is appended with host A's own IP address so that host B can reply to host A. This additional information is packed into a data structure called the IP header, which is the information at the beginning of an IP packet and contains the IP version, source IP address, destination IP address, life time, etc. The IP header is the information at the beginning of the packet.

![](1.jpg)

2. UDP: delivering packets to the application
IP is a very low-level protocol that is only responsible for sending packets to the other computer, but the other computer doesn't know which program to give the packets to, the browser or the king? Therefore, it is necessary to develop protocols that deal with applications based on IP, most commonly known as "User Datagram Protocol" (UDP).

One of the most important information in UDP is the port number, which is actually a number that every program that wants to access the network needs to bind a port number. The port number allows UDP to send the specified packet to the specified program, so IP sends the packet to the specified computer via the IP address information, and UDP distributes the packet to the correct program via the port number. Like the IP header, the port number is loaded into the UDP header, and the UDP header merges with the original packet to form a new UDP packet, which contains not only the destination port but also the source port number.

![](2.jpg)

When using UDP to send data, there are various factors that can cause packet errors, and while UDP can verify that the data is correct, UDP does not provide a mechanism to re-send packets that are incorrect, it just discards the current packet, and UDP has no way of knowing if it will reach its destination after sending it.

Although UDP does not guarantee data reliability, but the transmission speed is very fast, so UDP will be used in some areas of concern for speed, but less strict requirements for data integrity, such as online video, interactive games and so on.

3. TCP: delivering data to the application in its entirety
For applications such as browser requests, or emails that require reliability of data transmission, there are two problems with using UDP for transmission.

 The vulnerability of packets to loss during transmission.
Large files are split into many small packets for transmission, and these small packets go through different routes and arrive at the receiver at different times, while the UDP protocol does not know how to assemble these packets to reduce them to complete files.
Based on these two problems, we introduced TCP, a connection-oriented, reliable, byte-stream-based transport layer communication protocol. Compared to UDP, TCP has the following two characteristics:

TCP provides a retransmission mechanism for the case of packet loss.
TCP introduces a packet-sorting mechanism to ensure that disordered packets are combined into a complete file.
Like the UDP header, the TCP header contains the destination port and the local port number in addition to the sequence number used to sort the packets so that the receiving end can rearrange the packets by the sequence number.

Here is a look at the flow of a single packet transmission under TCP.

![](3.jpg)

The lifecycle of a complete TCP connection consists of three phases: "establish connection," "transfer data," and "disconnect.

First, the connection establishment phase. This phase establishes the connection between client and server through the "three handshakes". Connection-oriented refers to the preparation between the two ends before data communication begins. The three handshakes mean that when a TCP connection is established, the client and server send a total of three packets of data to confirm the establishment of the connection.
Next, the data transfer phase. In this phase, the receiving end needs to perform an acknowledgement operation on each packet, which means that after receiving the packet, the receiving end needs to send an acknowledgement packet to the sender end. So when the sending end sends a packet and does not receive a confirmation message back from the receiving end within a specified time, the packet is judged to be lost and a retransmission mechanism is triggered on the sending end. Similarly, a large file is split into many smaller packets during transmission, and when these packets reach the receiving end, the receiving end sorts them according to the sequence number in the TCP header to ensure that they are composed of complete data.
Finally, the disconnect phase. Once the data has been transmitted, the connection is terminated, involving a final phase of "four waves" to ensure that both parties are disconnected.
At this point you should understand that TCP sacrifices packet speed to ensure the reliability of the data transmission, because the "three handshakes" and "packet-checking mechanism" double the number of packets in the transmission process.

## Sum up

Data in the Internet is transmitted by packets of data, which can be easily lost or erroneous during transmission.
IP is responsible for sending the packets to the destination host.
UDP is responsible for sending the packets to the specific application.
TCP, on the other hand, ensures that data is transmitted intact, and its connection can be divided into three stages: establishing a connection, transferring data, and disconnecting.


