# Arp Spoofing

Last week, we have seen that ARP spoofing can be used to attack the confidentiality of a network. In this week, we will use a simplified version of an ARP packet to illustrate this behavior and try to defend against it.

A spoofed ARP packet is basically an unsollicited ARP reply for an IP address that is already in the network. The goal is to let the network believe that the attacker is the one that has the IP address specified in the packet. This can create a man in the middle attack.
### Step 1

The first goal of this assignment is to create an ARP spoofing packet in the simplified format. You can implement this in the function spoofArp(spoofIP). The argument passed to this function is the IP address that you want to impersonate.

### Step 2

The second goal of this assignment is to implement an ARP table for your router and detect attacks going on. An attack occurs when you observe a packet that tries to connect your MAC address to another IP or tries to claim an IP address that we know is already in use.
Do not save for requests, only for replies.

You have to implement the function that receives ARP packets: receiveArp(message). This function receives an ARP packet and returns one of three status codes:

|   OK - If the packet was handled by the system.         |
|---------------------------------------------------------|
|   IGNORE - If the packet was not meant for this system. |
|                                                         |
|   ATTACK - If an attack has been detected.              |
|---------------------------------------------------------|

Additionally, when the function receives a request of its own IP address, it returns not a status code, but the ARP reply instead.

The simplified packet looks as follows:

| Opcode: 1 byte    |   Sender Mac: 3 Bytes   |   Sender Ip: 2 Bytes    |   Target Mac: 3 Byes   |   Target IP  2 Bytes   |
|-------------------|-------------------------|-------------------------|------------------------|------------------------|

Opcode - 1 byte, 1 for request and 2 for reply.
    Rest of the fields are self explanatory.
    All fields are hexadecimal
