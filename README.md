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
    Java Classes	      Socket	            DatagramSocket	    RestController



TCP → Reliable (used in banking, messaging)
UDP → Fast (used in video streaming, games)
HTTP → Web communication (used in APIs)

will update few more soon





Java Developer






package com.debasish.arraypractice.oneDimensionalArray;

import java.util.LinkedList;
import java.util.Queue;

public class OneDimensionalArray98 {

    public static void main(String[] args) {

        // ======================================
        // PROBLEM 99: ROTTING ORANGES
        // ======================================

        // Step 1: create grid
        int[][] grid = {
                {2, 1, 1},
                {1, 1, 0},
                {0, 1, 1}
        };

        int rows = grid.length;
        int cols = grid[0].length;

        // queue stores row and column
        Queue<int[]> queue = new LinkedList<>();

        int fresh = 0;

        // Step 2: count fresh oranges
        // and store rotten oranges
        for (int i = 0; i < rows; i++) {

            for (int j = 0; j < cols; j++) {

                if (grid[i][j] == 2) {
                    queue.offer(new int[]{i, j});
                }

                if (grid[i][j] == 1) {
                    fresh++;
                }
            }
        }

        // Step 3: directions
        int[] rowDir = {-1, 1, 0, 0};
        int[] colDir = {0, 0, -1, 1};

        int minutes = 0;

        // Step 4: BFS traversal
        while (!queue.isEmpty() && fresh > 0) {

            int size = queue.size();

            for (int i = 0; i < size; i++) {

                int[] current = queue.poll();

                int row = current[0];
                int col = current[1];

                // check 4 directions
                for (int j = 0; j < 4; j++) {

                    int newRow = row + rowDir[j];
                    int newCol = col + colDir[j];

                    // valid fresh orange
                    if (newRow >= 0 && newCol >= 0
                            && newRow < rows && newCol < cols
                            && grid[newRow][newCol] == 1) {

                        // make rotten
                        grid[newRow][newCol] = 2;

                        // reduce fresh count
                        fresh--;

                        // add to queue
                        queue.offer(new int[]{newRow, newCol});
                    }
                }
            }

            minutes++;
        }

        // Step 5: print result
        if (fresh == 0) {
            System.out.println("Minutes Required: " + minutes);
        } else {
            System.out.println("Not Possible");
        }

    }
}
