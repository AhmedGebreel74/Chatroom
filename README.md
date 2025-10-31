# Simple Chatroom (Go RPC)

This project is a simple client-server chatroom built using Go RPC (Remote Procedure Calls).
It allows multiple clients to connect to a central server, send messages, and view the shared chat history in real time.

🎥 ## Demo Video

You can watch a full demo of the chatroom running here:
👉 **[Watch the Demo Video](https://drive.google.com/file/d/1Y6T3QjCTLXmBgZ8hRFSVpHWB_IYk8zhb/view?usp=sharing)**

---

## Overview

### 🖥️ Server

* Acts as the central message coordinator.
* Receives and stores all messages from connected clients.
* Returns the full chat history every time a new message is received.
* Uses a mutex lock to safely handle multiple clients sending messages at the same time.
* Can be stopped manually by typing `exit` in the terminal.

### 💬 Client

* Connects to the server through an RPC connection.
* Lets users type messages and see the entire conversation.
* Runs continuously until the user types `exit` or presses `Ctrl + C`.
* Handles errors if the server goes down and shows a clear message to the user.

---

### 📂 Files

├── server.go # Go RPC server implementation ├── client.go # Go RPC client implementation └── README.md # Project documentation


---

## 🚀 How to Run

### 1️⃣ Start the Server

In one terminal:

```bash
go run server.go
By default, the server runs on port 1234. To use a different port:
