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


