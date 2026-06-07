---
tags: [book, os, operating-systems, book-os-concepts, processes, networking]
aliases: ["3.6.1", "Sockets"]
---

# Sockets

1) A **socket** is defined as an endpoint for communication. A pair of processes communicating over a network employ a pair of sockets—one for each process. 
2) A socket is identified by an IP address concatenated with a port number. In general, sockets use a client-server architecture. The server waits for incoming client requests by listening to a specified port.
3) Once a request is received, the server accepts a connection from the client socket to complete the connection. Servers implementing specific services (such as telnet, FTP, and HTTP) listen to well-known ports (a telnet server listens to port 23; an FTP server listens to port 21; and a Web, or HTTP, server listens to port 80). All ports below 1024 are considered _well known;_ we can use them to implement standard services.
4) When a client process initiates a request for a connection, it is assigned a port by its host computer. This port is some arbitrary number greater than 1024. For example, if a client on host X with IP address 146.86.5.20 wishes to establish a connection with a Web server (which is listening on port 80) at address 161.25.19.8, host X may be assigned port 1625. The connection will consist of a pair of sockets: (146.86.5.20:1625) on host X and (161.25.19.8:80) on the Web server. This situation is illustrated in Figure 3.18. The packets traveling between the hosts are delivered to the appropriate process based on the destination port number.

![[Pasted image 20260604185646.png]]

5) All connections must be unique. Therefore, if another process also on host X wished to establish another connection with the same Web server, it would be assigned a port number greater than 1024 and not equal to 1625. 
6) This ensures that all connections consist of a unique pair of sockets.
7) Java provides three different types of sockets. **Connection-oriented (TCP) sockets** are implemented with the Socket class. **Connectionless (UDP) sockets** use the DatagramSocket class. Finally, the MulticastSocket class is a subclass of the DatagramSocket class. A multicast socket allows data to be sent to multiple recipients.
8) Our example describes a date server that uses connection-oriented TCP sockets. The operation allows clients to request the current date and time from the server. The server listens to port 6013, although the port could have any arbitrary number greater than 1024. When a connection is received, the server returns the date and time to the client.
9) The date server is shown below. The server creates a ServerSocket that specifies it will listen to port 6013. The server then begins listening to the port with the accept() method. The server blocks on the accept() method waiting for a client to request a connection. When a connection request is received, accept() returns a socket that the server can use to communicate with the client.
10) The details of how the server communicates with the socket are as follows. 
	- The server first establishes a **PrintWriter** object that it will use to communicate with the client. 
	- A **PrintWriter** object allows the server to write to the socket using the routine print() and println() methods for output. 
	- The server process sends the date to the client, calling the method println(). Once it has written the date to the socket, the server closes the socket to the client and resumes listening for more requests.
11) A client communicates with the server by creating a socket and connecting to the port on which the server is listening. We implement such a client in the Java program shown in Figure 3.20. 
12) The client creates a Socket and requests a connection with the server at IP address 127.0.0.1 on port 6013. Once the connection is made, the client can read from the socket using normal stream I/O statements. 
13) After it has received the date from the server, the client closes the socket and exits. 
14) The IP address 127.0.0.1 is a special IP address known as the **loopback**. 
15) When a computer refers to IP address 127.0.0.1, it is referring to itself. This mechanism allows a client and server on the same host to communicate using the TCP/IP protocol. The IP address 127.0.0.1 could be replaced with the IP address of another host running the date server. In addition to an IP address, an actual host name, such as _[www.westminstercollege.edu](http://www.westminstercollege.edu)_, can be used as well.

```java
import java.net.*;
import java.io.*;

public class DateServer
{
	public static void main(String[] args) {
		try {
			ServerSocket sock = new ServerSocket(6013);

			// now listen for connections
			while (true) {
				Socket client = sock.accept();

				PrintWriter pout = new
					PrintWriter(client.getOutputStream(), true);

				// write the Date to the socket
				pout.println(new java.util.Date().toString());

				// close the socket and resume
				// listening for connections
				client.close();
			}
		}
		catch (IOException ioe) {
			System.err.println(ioe);
		}
	}
}
```

16) Communication using sockets—although common and efficient—is considered a low-level form of communication between distributed processes. One reason is that sockets allow only an unstructured stream of bytes to be exchanged between the communicating threads.
17) It is the responsibility of the client or server application to impose a structure on the data. In the next two subsections, we look at two higher-level methods of communication: remote procedure calls (RPCs) and pipes.

## Related

- [[> Communication in Client-Server Systems]] — back to the sub-topic MOC
- [[Remote Procedure Calls]] — a higher-level alternative to raw sockets
- [[Pipes]] — the other higher-level communication method
- [[TCP]] — the connection-oriented protocol behind TCP sockets
