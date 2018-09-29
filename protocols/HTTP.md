## HTTP

*TCP* is a connection-oriented protocol while *UDP* is connectionless.

**IP datagram:** The IP layer of the network stack  provides connectionless service by transfering datagrams. All of which are independent of each other and any information about datagram associations must be provided by a higher network layer

  > What is a datagram?
  > Basic unit that is transfered over the network. In short `headers + payload = datagram`.

IP also supplies a checksum including its own header (which in turn includes the source and destination of the datagram)

IP also handles routing and breaking up large datagrams into smaller ones and reassembling them at the other end.

**UDP:**
- Built on top of the IP layer.
- Connectionless and unreliable packet delivery just as IP
- It adds a checksum for contents of the datagram and the port numbers to the IP layer.

**TCP**
- Another layer built on top of IP
- Focussed on accuracy and provides ordered delivery of stream of packets
- Provides logic to give a reliable connection oriented protocol on top of IP
- It provides a virtual circuit for two processes to communicate with each other

### Adressing Scheme
Schemes are used to locate servies and devices over a network.

#### IPV4
- 32 bit int written on the network interface card of a device
- Format: 4 bytes in decimal separated by a "." (like 127.0.0.1)
