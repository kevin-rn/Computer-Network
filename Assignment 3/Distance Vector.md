# Dynamic Routing - Distance Vector

Dynamic routing makes use of messages to learn where to route packets to get them to their destination. There are two different types of dynamic routing algorithms: Distance Vector and Link State. Distance vector routing algorithms only gather data from their neighbour (directly connected) routers, while link state gathers data from all routers in the network. Because of this difference both types of algorithms have different advantages and disadvantages.
Distance Vector

Distance vector algorithms only know the state of their neighbours. They use propagation to spread knowledge about distant routers. Each router slowly learns more about the network by gossiping newly discovered routers.

Advantages compared to link state algorithms:
- Low bandwidth requirements due to local sharing
- Small packets
- No flooding

Disadvantages compared to link state algorithms:
- Based on local knowlegde instead of global
- Converges slowly e.g. takes longer to propegate broken links

An example of a distance vector algorithm is RIP.

# Programming exercise

In this programming exercise, you will implement a distance vector routing table. A template will be provided with the correct function signatures. A basic test case can be found under the Test tab, but you are highly encouraged to write your own test cases to make sure your implementation is correct.

You are given the following functions (exact method signatures can be found in the code template):

### Constructor (Router Identifier: str) -> void

This function is used to store the router indentifier.

Arguments:
- Router Identifier: Name of the router. This argument is a 4 character hexadecimal string.

Returns: This function should not return anything

### Process Packet (Packet: str) -> str

This function is given to parse the raw packet and detect the type of packet and subsequently call the corresponding function .e.g Process Data Packet or Process Routing Packet. We provide this function because learning how to parse strings is not an objective of this course. You shouldn’t have to touch or understand this function to complete the assignment.

Arguments:
- Packet: Incoming network packet. The format of the packet is specified below.

### Network packet format: (For explanation only)

D <source> <destination> <message>

Where:
- Source: The original source of the packet. (4 character hexadecimal string)
- Destination: The destination of the packet. (4 character hexadecimal string)
- Message: The actual message. (arbitrary length hexadecimal string)

R <identifier> <latency> <n> <identifier-n> <latency-n> ...

Where:
- identifier: The identifier of the neighbour router. (4 character hexadecimal string)
- latency: The network latency to the neighbour router measured from your router. (number)
- n: The number of route entries in the neighbours routing table. (number)
- identifier-n: The identifier of the router in the routing table. (4 character hexadecimal string)
- latency-n: The latency of the router in the routing table measured from the neighbour router. (number)

Examples of valid packets are:

D aaaa bbbb 1234567890abcdef
R cccc 20 2 bbbb 5 2222 25

Returns: This function should return the output from either the Process Data Packet function or the Process Routing Packet function. If an invalid type is provided the function should return ERROR_INVALID_TYPE, which is mapped to ‘INVALID TYPE’ in the code.

### rocess Data Packet (Source: str, Destination: str, Message: str) -> str

This function processes data packets and looks up the route and latency in the routing table(s) and returns these. If a message is directed to the current router, the router should return MESSAGE_THANK_YOU, which is mapped to ‘THANK YOU’ in the code.

Arguments:
- Source: Original source of message. This argument is a 4 character hexadecimal string.
- Destination: Final destination of message. This argument is a 4 character hexadecimal string.
- Message: Fake message.

Returns: This function should return the neighbour router which can route to the desired destination and the total latency to the final destination (format provided below). If no valid route can be found the function should return ERROR_NO_ROUTE, which is mapped to ‘NO ROUTE’ in the code. If a message is directed to the current router, the router should return MESSAGE_THANK_YOU, which is mapped to ‘THANK YOU’ in the code.

<identifier> <total latency>
cccc 25

### Process Routing Packet (Router Identifier: str, Latency: int, Routes: List[Route]) -> str

This function processes routing packets and updates the routing table(s) with new information.

Arguments:
- Router Identifier: Name of the neighbour router. This argument is a 4 character hexadecimal string.
- Latency: The network latency to the neighbour router measured from your router.
- Routes: List of routes from the neighbour router.

### Route (Router Identifier: str, Latency: int)

Where:
- Router Identifier: The identifier of the router in the routing table of your neighbour. This argument is a 4 character hexadecimal string.
- Latency: The latency of the router in the routing table of your neighbour measured from the neighbour router.

Returns: This function should return MESSAGE_ROUTE_RECEIVED (which is mapped to ‘RECEIVED’ in the code) for every routing packet received

### Pointers

    If two routes have the same total latency return the one which is the newest.
    New routing information can be provided at any time.
    Routers can update their latency or the latency of their neighbours. Since this makes the exercise more complex, we have designed the spec tests in such a way that it is possible to pass the assignment (score 90/100) without having this functionality. If you do want to go for the full 100/100, here are some hints for different datastructures that can be used:

Less efficient, but easier (work is done on Data Packet instead of Routing Packet)

Map[Destination: str, Map[Forwarding Router: str, Latency: int]] (Routes table)
Map[Forwarding Router: str, Latency: int] (Neighbours table)

More efficient, but harder (work is done on Routing Packet instead of Data Packet)

Map[Destination: str, Route (Forwarding Router: str, Total latency: int)] (Routing table lookup)
Map[Destination: str, Map[Forwarding Router: str, Latency: int]] (Routes table)
Map[Forwarding Router: str, Latency: int] (Neighbours table)

### Assumptions

    You can assume that our input is valid and non-malicious.
    You can assume that the network graph stays static, but new nodes can still be discovered, since you don’t have a view of the entire network.
    You can assume the latency is the same in both directions
    
_____________________________________________________________________________________________________________________________________
__Template:__
```java
package weblab;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

/**
 * Routing table for dynamic distance vector routing.
 * It holds a list of routes that can be queried to find the router which can reach the destination.
 */
class RoutingTable {

  public static final String DATA_PACKET_TYPE = "D";

  public static final String ERROR_INVALID_TYPE = "INVALID TYPE";

  public static final String ERROR_NO_ROUTE = "NO ROUTE";

  public static final String MESSAGE_THANK_YOU = "THANK YOU";

  public static final String MESSAGE_ROUTE_RECEIVED = "RECEIVED";

  public static final String ROUTING_PACKET_TYPE = "R";

  /**
   * Constructor to take in the router's identifier.
   *
   * @param routerIdentifier Identifier of the router
   */
  public RoutingTable(String routerIdentifier) {
  // TODO
  }

  /**
   * GIVEN: This processes the packets for you and calls the routing or data packet processors accordingly.
   *
   * @param packet Network packet supplied by our test cases.
   * @return String of the reply from the incoming packet.
   */
  public String processPacket(String packet) {
    String[] packetParts = packet.split(" ");
    String packetType = packetParts[0];
    switch(packetType) {
      case ROUTING_PACKET_TYPE:
        int numberOfRoutes = Integer.parseInt(packetParts[3]);
        ArrayList<Route> routes = new ArrayList<>();
        int index = 4;
        for (int i = 0; i < numberOfRoutes; i++) {
          routes.add(new Route(packetParts[index], Integer.parseInt(packetParts[index + 1])));
          index += 2;
        }
        return processRoutingPacket(packetParts[1], Integer.parseInt(packetParts[2]), routes);
      case DATA_PACKET_TYPE:
        return processDataPacket(packetParts[1], packetParts[2], packetParts[3]);
      default:
        return ERROR_INVALID_TYPE;
    }
  }

  /**
   * Process the data packet, this function is called automatically from processPacket.
   *
   * @param source      Original source of message.
   * @param destination Final destination of message.
   * @param message     Fake message.
   * @return The neighbour router which can route to the desired destination and the total latency
   * to the final destination. If no valid route can be found the function should return ERROR_NO_ROUTE,
   * which is mapped to 'NO ROUTE' in the code. If a message is directed to the current router,
   * the router should return MESSAGE_THANK_YOU, which is mapped to 'THANK YOU' in the code.
   */
  private String processDataPacket(String source, String destination, String message) {
  // TODO
  }

  /**
   * Process the router packet, this function is called automatically from processPacket.
   *
   * @param routerIdentifier Originating router id given in hexadecimal.
   * @param latency          Latency from the origin router.
   * @param routes           The neighbours of the origin router given as a list of Route objects.
   * @return MESSAGE_ROUTE_RECEIVED (which is mapped to 'RECEIVED' in the code).
   */
  private String processRoutingPacket(String routerIdentifier, int latency, List<Route> routes) {
  // TODO
  }
}

/**
 * GIVEN: Route entry used for storing route information
 * This is used for representing the forwarding routes from the neighbouring router
 */
class Route {

  String forwardingRouter;

  int latency;

  public Route(String forwardingRouterIdentifier, int latency) {
    this.forwardingRouter = forwardingRouterIdentifier;
    this.latency = latency;
  }
}
```
