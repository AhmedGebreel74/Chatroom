# Simple Chatroom (Go RPC)

This project is a simple **client-server chatroom** built using **Go RPC (Remote Procedure Calls)**.  
It allows multiple clients to connect to a central server, send messages, and view the shared chat history in real time.

---

## Overview

### 🖥️ Server
- Acts as the central message coordinator.  
- Receives and stores all messages from connected clients.  
- Returns the full chat history every time a new message is received.  
- Uses a **mutex lock** to safely handle multiple clients sending messages at the same time.  
- Can be stopped manually by typing `exit` in the terminal.

### 💬 Client
- Connects to the server through an RPC connection.  
- Lets users type messages and see the entire conversation.  
- Runs continuously until the user types `exit` or presses `Ctrl + C`.  
- Handles errors if the server goes down and shows a clear message to the user.

---

## Files

```
├── server.go   # Go RPC server implementation
├── client.go   # Go RPC client implementation
└── README.md   # Project documentation
```

---

## How to Run

### 1️⃣ Start the Server
In one terminal:
```bash
go run server.go
```
By default, the server runs on port **1234**.  
To use a different port:
```bash
CHAT_PORT=8080 go run server.go
```

### 2️⃣ Start the Client
In another terminal:
```bash
go run client.go
```
If you’re using a custom port:
```bash
CHAT_ADDR=localhost:8080 go run client.go
```

You can start multiple clients to simulate different users chatting at the same time.

---

## Example Output

**Server:**
```
Chat server running on port 1234...
[12:00:01] Ali: Hello everyone!
[12:00:10] Sara: Hi Ali!
```

**Client:**
```
Enter your name: Ali
Welcome Ali! You've joined the chat.
Enter message (or 'exit' to quit): Hello everyone!

--- Chat History ---
Ali: Hello everyone!
--------------------
Enter message (or 'exit' to quit): exit
Bye!
```

---

## 🎥 Demo Video

You can watch a full demo of the chatroom running here:  
👉 [**Watch the Demo Video**](#)  
*(Replace `#` with your actual link — e.g., YouTube, Google Drive, or GitHub upload.)*

---

## 🧠 Documentation

### 1. System Design
This project follows a **Client–Server Architecture** using **Go’s RPC package**:
- The **server** runs continuously and listens for incoming RPC connections.
- Each **client** connects to the server and communicates by calling remote methods.  
  When a client sends a message, the server stores it and sends back the entire chat history.

### 2. Server Explanation (`server.go`)
- The server defines a struct `ChatServer` which holds:
  ```go
  type ChatServer struct {
      mu      sync.Mutex
      history []ChatMessage
  }
  ```
- The `SendMessage` method:
  - Validates the incoming message.
  - Appends it to the shared `history` slice.
  - Returns a **copy** of the full chat history to the client.
  - Uses a **mutex lock (`mu.Lock()`)** to ensure thread-safe access when multiple clients send messages simultaneously.
- The `GetHistory` method simply returns all previous messages without modifying anything.
- The server listens for connections using:
  ```go
  listener, _ := net.Listen("tcp", ":1234")
  rpc.ServeConn(conn)
  ```
- The server can be closed by typing `exit` in the terminal.

### 3. Client Explanation (`client.go`)
- The client connects using:
  ```go
  client, err := rpc.Dial("tcp", "localhost:1234")
  ```
- It asks for the user’s name and allows them to type messages in a loop.
- After each message, the client calls the server method:
  ```go
  client.Call("ChatServer.SendMessage", msg, &history)
  ```
- The server sends back the full chat history, which the client prints in a clear format.
- The program continues until the user types `exit`.

### 4. Data Structure
The `ChatMessage` struct defines the message format:
```go
type ChatMessage struct {
    Author    string
    Text      string
    Timestamp time.Time
}
```
Each message includes:
- **Author** – the sender’s name  
- **Text** – the actual message  
- **Timestamp** – the exact time when the message was sent

### 5. Error Handling
- If the server isn’t running, the client prints:
  ```
  The server might be down. Try again later.
  ```
  and exits gracefully.  
- If the connection breaks during a session, the client also stops safely instead of crashing.

### 6. Improvements (Optional)
This project can be extended easily to include:
- Saving messages to a file or database.  
- Supporting multiple chatrooms.  
- Adding usernames and authentication.  
- Using WebSockets for real-time updates instead of RPC.

---

## 👨‍💻 Author

**Name:** [Your Name]  
**Assignment:** 04 – Simple Chatroom  
**Course:** Distributed Systems / Computer Networks  

---
