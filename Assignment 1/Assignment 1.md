## Lab Assignment 1
### Introduction

This assignment focuses on the data link layer from the hybrid computer networks model used in the book Computer Networks by Andrew Tanenbaum and David J. Wetherall. This layer is also present in the Open Systems Interconnection (OSI) model.
The Data Link Layer

In the networking model, the data link layer is located between the network layer and the physical layer. The data link layer receives packets from the network layer and splits these up into frames. It passes these frames to the physical layer, which in turn converts them into a bit-stream that is transmitted over the physical medium. On the receiving end, the reverse process takes place. Similar to the other layers in the networking stack, the data link layer adds meta-data in the form of headers and trailers. The meta-data added by the data link layer typically contains the following information:

   * Destination: The address of the device that must receive this data.
   * Source: The address of the device sending this data.
   * Length: How many bytes of data is being sent in this frame.
   * Payload: The data to be transmitted. This data can be a complete or partial packet obtained from the network layer.
   * Redundant bits: A bit sequence that is computed based on the value of the other fields. 
                    These bits are used to detect or correct errors that occurred during transmission.

One important job of the data link layer is to make sure that all frames are received without errors. This can be done by either retransmitting corrupted frames detected using error detection codes, or recovering the original data on the side of the receiver using error correction codes.

In this assignment, you will learn about hamming distances that determine the error detection/correction properties of a code. You will also implement a very common error detection algorithm, Cyclic Redundancy Check (CRC), and a very common error correction algorithm, Hamming Code.
