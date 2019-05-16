Backward-Learning

In the data-link layer, hubs, bridges, and switches are the main type of devices you are going to encounter. The function of these devices is to eliminate the need for shared access mediums on wired network connections. Before the introduction of these devices, clients had to wait their turn before they could send something on the shared channel. With the introduction of hubs, it was possible for each client to have a dedicated channel to the hub that didn’t go down when other computers were offline. Although the hub was a step forward, it still had the disadvantage of blocking the network when a packet was sent by broadcasting it to every client connected to the hub. Bridges and switches solved this problem by keeping track of where clients are located on the switch, in a table. This table stores mappings from MAC address to port number. To learn the location of the clients, bridges and switches use an algorithm called backward learning. This algorithm automatically starts learning about all the clients when they become active on the network.

For more information see section 4.8.2 of the book.
Programming exercise

In this programming exercise, you implement the software that runs on our made-up simplified switch. This switch receives frames on multiple links. Your code needs to forward these frames on the appropriate links. A template will be provided with the correct function signature. A basic test case can be found under the Test tab, but you are highly encouraged to write your own test cases to make sure your implementation is correct.

The core task of the switch is to forward incoming frames on the correct links. Initially, it is unknown how the machines are connected to the different ports. In the backward-learning algorithm, when the location of a machine is unknown, the message is flooded. If a message is sent out on multiple links, these links should appear in ascending order in the output. Your program should also take into account machines connected to the switch via a hub. From the perspective of the switch, this means these machines are connected to the same port.

The bytes that are part of the frame are encoded in hexadecimal notation, which means every two characters correspond to one byte.

The frame has the following format:

aaaabbbbxxxxxxxxxxxxxxxxxxxx

Where aaaa is a hexadecimal representation of the source computer
Where bbbb is a hexadecimal representation of the destination computer
Where xxxxxxxxxxxxxxxxxxxx is a hexadecimal representation of the message

We use a fixed length for the message of 20 hex values, but it shouldn’t matter in your implementation.

The output should have the following format: A list of all ports on which the frame is sent out by your switch. If the frame is not sent out on any links, the output should be an empty list.

An example of an output for a broadcast to 4 ports, where the sender is on port 1 would be:

[0, 2, 3]

Assumptions

    You may assume that you are always allowed to transmit on a link, even if multiple machines are connected to it
    You may assume that there are no collisions
    You may assume that frames are forwarded to their destination, even if their checksum indicates a transmission error.
