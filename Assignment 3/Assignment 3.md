# Lab Assignment 3
### Introduction

This assignment focuses on the network layer from the hybrid computer networks model used in the book Computer Networks by Andrew Tanenbaum and David J. Wetherall. This layer is also present in the Open Systems Interconnection (OSI) model.
The Network Layer

In computer networking, routers are responsible for forwarding data packets between computer networks. A router is connected, usually via cables, to at least two different networks. When it receives a new packet from one of the networks, it reads the packet’s destination address (usually an IP address) and attempts to forward it to the appropriate network, so that it will (eventually, hopefully) reach its destination.

To successfully forward a data packet towards its destination, the router needs to know which destinations can be reached through the different networks to which it is connected. To determine which destinations can be reached a router can make use of three different kinds of algorithms:
- Static routing
- Dynamic routing - distance vector
- Dynamic routing - link state

This assignment will go through all of them and help you understand their advantages and disadvantages
