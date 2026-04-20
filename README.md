# networking_in_java
Common Networking Protocols in Java

1. TCP (Transmission Control Protocol)

Definition: TCP is a connection-oriented protocol. It ensures reliable communication (data arrives in order, no loss).

Java Concept
Uses: Socket → client
      ServerSocket → server

Client sends a message to server → server receives and responds.

SpringBoot example :


     package com.example.network;

     import java.io.*;
     import java.net.*;

    public class TcpExampleApplication {

    public static void main(String[] args) throws Exception {

        // ---- SERVER LOGIC ----
        // Create server socket on port 5000
        ServerSocket server = new ServerSocket(5000);
        System.out.println("Server started...");

        // Accept client connection
        Socket socket = server.accept();
        System.out.println("Client connected");

        // Read data from client
        BufferedReader reader = new BufferedReader(
                new InputStreamReader(socket.getInputStream()));
        String message = reader.readLine();
        System.out.println("Received: " + message);

        // Send response to client
        PrintWriter writer = new PrintWriter(socket.getOutputStream(), true);
        writer.println("Hello from Server");

        socket.close();
        server.close();

        // ---- CLIENT LOGIC ----
        Socket client = new Socket("localhost", 5000);

        PrintWriter clientWriter = new PrintWriter(client.getOutputStream(), true);
        clientWriter.println("Hello Server");

        BufferedReader clientReader = new BufferedReader(
                new InputStreamReader(client.getInputStream()));
        System.out.println("Server says: " + clientReader.readLine());

        client.close();
       }
    }


Explanation:
Server waits for client (accept())
Client connects using IP + port
Data flows using input/output streams


2. UDP (User Datagram Protocol)

UDP is connectionless and faster but not reliable.

Uses:  DatagramSocket 
       DatagramPacket

Client sends packet → server receives packet (no connection setup required)

Example :

    package com.example.network;

    import java.net.*;

    public class UdpExampleApplication {

    public static void main(String[] args) throws Exception {

        // ---- SERVER ----
        DatagramSocket serverSocket = new DatagramSocket(6000);

        byte[] receiveBuffer = new byte[1024];
        DatagramPacket receivePacket = new DatagramPacket(receiveBuffer, receiveBuffer.length);

        serverSocket.receive(receivePacket); // receive data
        String receivedData = new String(receivePacket.getData());
        System.out.println("Received: " + receivedData);

        // ---- CLIENT ----
        DatagramSocket clientSocket = new DatagramSocket();

        String msg = "Hello UDP Server";
        byte[] sendData = msg.getBytes();

        InetAddress ip = InetAddress.getByName("localhost");
        DatagramPacket sendPacket = new DatagramPacket(sendData, sendData.length, ip, 6000);

        clientSocket.send(sendPacket);

        clientSocket.close();
        serverSocket.close();
        }
    }


Explanation:
No connection establishment is required.
Also faster but packets may be lost.


3. HTTP (HyperText Transfer Protocol)
   Mainly HTTP is used for web communication (REST APIs).

Uses:
Spring Boot (@RestController)
HttpURLConnection or WebClient

Client sends GET request → server returns JSON response

Example:

    package com.example.network;
    import org.springframework.boot.*;
    import org.springframework.boot.autoconfigure.*;
    import org.springframework.web.bind.annotation.*;

    @SpringBootApplication
    @RestController
    public class HttpExampleApplication {

    public static void main(String[] args) {
        SpringApplication.run(HttpExampleApplication.class, args);
    }

    // Simple GET API
    @GetMapping("/hello")
    public String hello() {
        return "Hello from HTTP Server";
       }
    }

Runs on port 8080 by default
Access using browser → http://localhost:8080/hello


Difference Table (Interview Important)

Feature     	TCP                 	UDP	                HTTP
Type	            Connection-oriented	Connectionless   	    Application protocol
Reliability	      High	                  Low	                High (built on TCP)
Speed	            Slower	            Faster	          Medium
Use Case	      Chat, file transfer	Streaming, gaming	    Web APIs
Java Classes	Socket	            DatagramSocket	    RestController
