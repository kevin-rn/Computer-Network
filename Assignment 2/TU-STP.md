# TU-STP

Devices in the data-link layer are normally connected redundantly to make sure that physical failures will not cause a network outage. A good example of this is a switch. When there are two cables connecting two switches, if one fails the other one can still carry the traffic (Although possibly at a reduced rate). However, when there are multiple routes to a destination (Also called loops), a broadcast storm can occur when packets are sent to the entire network. This happens because, with the loop, there is always a path for the traffic to go (that isn’t the incoming link). To solve this problem, higher-end switches make use of a protocol called Spanning Tree Protocol (STP). This protocol constructs a view of the network and creates a tree out of the network graph, without any loops. The extra redundant links are temporarily disabled to accomplish this. When one of the links in the network has failures, the STP protocol can automatically reconfigure the network to use the redundant links.

The regular STP has two main steps:
1. Determine the root switch of the network
2. Calculate the best path (based on e.g. hops, latency, speed or bandwidth) from the current switch to the root switch

For more information see section 4.8.3 of the book.

# Programming exercise

In this programming exercise, you are tasked to implement software that will implement our TU-STP protocol. This protocol is a simplified version of the regular STP: the root switch is determined up front and the network is static (i.e. no links are added or removed).

You should implement a Switch class. The constructor receives the MAC of this switch, that of the root and the number of ports the switch has. You should initialize the class variables here, but not send any messages.
Furthermore every switch has a receive function to receive messages.

To create the tree, the root switch of the network will receive a “hop” packet. This packet should be broadcasted, so other switches can use this to determine their distance to the root as well as the port they should send data to when trying to reach the root.

As the network may be cyclic, a switch may receive the “hop” packet more than once. Your code needs to be able to determine the ports that should be blocked to create the STP tree. You may also need to unblock links at a later point, if it turns out they should be part of the tree after all.

After the initial “hop” packet is sent to the root (which is done in Library.init_test()), the tests can send packets to any switch. The switch should send the packet over the correct port to reach the destination of the packet. If a switch does not know where to send a packet, it should send it to the root. The root should drop packets when it does not know where to send them.
Sending messages

A library that emulates the network is provided. You should use this to send data over links to other switches. The only function you should use is the following:
Library.send(int senderMac, int senderPort, String frame)
This sends a message with the given frame from senderMac over its given senderPort.

The bytes that are part of the frame are encoded in hexadecimal notation, which means every two characters correspond to one byte.
Frame formatting

The messages sent between the switches should be formatted as follows:

1 byte Flag -> 0 for a normal packet, 1 for the initial ‘hop’ packet (you can use other flags your own messages)
2 bytes source mac
2 bytes destination mac
(if hopping packet) 2 bytes hop-count

So, a valid frame could look like 01BBBBAAAA0002, which would correspond to a ‘hop’ packet (the flag is 01) send to switch with mac AAAA, coming from the switch with mac BBBB, and a hop-count of 2.

Another valid frame would be 00AAAACCCC, which would correspond to a regular packet that comes from AAAA and has to be routed to CCCC.

The initial “hop” packet that is sent to the root is the following: 010000FFFF0000 (hop packet with hop count 0). This is received on link -1.

Note: you should not deviate from this frame formatting, otherwise the tests will fail.
Testing

In the library there is a function that returns the entire backlog of messages sent (for testing purposes). The backlog starts from the first regular packet (i.e. the hop packets are not recorded).

The backlog is a list consisting of strings in the format: [Source][SourcePort]-[The frame].
For example: aaaa01-00bbbbcccc would be a packet coming from switch aaaa on port 1, with the frame as specified in the frame format.
Visible test

One test network has been provided. It represents the network in this image:

Note that the link between dddd and 1111 is a dead link: both switches have a shorter path to the root.

## Useful commands

### Java

To process the frame string, you can use the following:


int flag = Integer.parseInt(frame.substring(0, 2), 16);
int source = Integer.parseInt(frame.substring(2, 6), 16);
int dest = Integer.parseInt(frame.substring(6, 10), 16);
(and if applicable)
int hopcount = Integer.parseInt(frame.substring(10, 14), 16);



To create a frame, you can use:


String newframe = String.format(“%02x”,flag) + String.format(“%04x”, source) + String.format(“%04x”, dest) + String.format(“%04x”, hopcount);


### Python

To process the frame string, you can use the following:


flag = int(frame[0:2], 16)
source = int(frame[2:6], 16)
dest = int(frame[6:10], 16)
(and if applicable)
hopcount = int(frame[10:14], 16)



To create a frame, you can use:

newframe = str(format(flag, ‘02x’)) + str(format(source, ‘04x’)) + str(format(dest, ‘04x’)) + str(format(hopcount, ‘04x’))

_____________________________________________________________________________________________________________________________________

__Template:__
```java
import java.util.*;

class Solution {

  private int ports;
  private int mac;
  private int root;

  /**
   * Switch constructor.
   *
   * @param mac_root The MAC of the root switch
   * @param mac The MAC of this switch
   * @param numberOfPorts The number of ports of the switch
   */
  public Solution(int macRoot, int mac, int numberOfPorts) {
    root = macRoot;
    mac = mac;
    ports = numberOfPorts;

  }

  /**
   * Gets called for every incoming message to the switch.
   *
   * @param link The port on which the message came in
   * @param frame The message that came in
   */
  public void receive(int link, String frame) {
  
  }
}
```
 
__Library:__
```java
import java.util.LinkedList;
import java.util.List;

class Library {

  private static Map<Integer, Solution> switches = new HashMap<>();

  private static Map<Integer, Map<Integer, Integer>> portMapping = new HashMap<>();

  private static Map<Integer, Map<Integer, Integer>> switchMapping = new HashMap<>();

  private static List<String> commands = new LinkedList<>();

  private static int rootswitch;

  public static void cleanTest() {
    switches = new HashMap<>();
    portMapping = new HashMap<>();
    switchMapping = new HashMap<>();
    commands = new LinkedList<>();
  }

  public static void send(int senderMac, int senderPort, String frame) {
    commands.add(String.format("%04x", senderMac) + String.format("%02x", senderPort) + "-" + frame);
    if (portMapping.containsKey(senderMac)) {
      if (portMapping.get(senderMac).get(senderPort) == null)
        return;
      int switchId = portMapping.get(senderMac).get(senderPort);
      if (switchMapping.get(switchId) == null || switches.get(switchId) == null)
        return;
      int receivingPort = switchMapping.get(switchId).get(senderMac);
      switches.get(switchId).receive(receivingPort, frame);
    } else {
      System.out.println("Sender MAC doesn't exist");
    }
  }

  public static void init_test() {
    switches.get(rootswitch).receive(-1, "010000FFFF0000");
    commands = new LinkedList<>();
  }

  public static List<String> get_calls() {
    return commands;
  }

  public static void add_switch(Solution swi, int macRoot, int macSwitch) {
    rootswitch = macRoot;
    switches.put(macSwitch, swi);
  }

  public static void add_connection(int sw1, int port1, int sw2, int port2) {
    portMapping.putIfAbsent(sw1, new HashMap<>());
    portMapping.putIfAbsent(sw2, new HashMap<>());
    switchMapping.putIfAbsent(sw1, new HashMap<>());
    switchMapping.putIfAbsent(sw2, new HashMap<>());
    portMapping.get(sw1).put(port1, sw2);
    portMapping.get(sw2).put(port2, sw1);
    switchMapping.get(sw1).put(sw2, port1);
    switchMapping.get(sw2).put(sw1, port2);
  }
}
```

