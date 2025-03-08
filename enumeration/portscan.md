TCP
a host sends a TCP SYN packet to a server on a destination port. If the destination port is open, the server responds with a SYN-ACK packet and the client host sends an ACK packet to complete the handshake. If the handshake completes successfully, the port is considered open.

netcat - not traditional port scanner

We can demonstrate this by running a TCP Netcat port scan on ports 3388-3390. We'll use the -w option to specify the connection timeout in seconds, as well as -z to specify zero-I/O mode, which is used for scanning and sends no data.

```
nc -nvv -w 1 -z 192.168.50.152 3388-3390
```