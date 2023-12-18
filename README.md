# peer-to-peer-chat

# Network Chat System

## Overview

This project implements a TCP-based network chat system where each chat program acts as both a client and a server. The program allows users to join a chat network, send and receive messages, and interact with other connected peers. The implementation uses pthreads for multi-threading, Linux system calls for network operations, and follows the TCP/IP networking stack.
This is a simple network chat system implemented using TCP/IP protocol in Java. It allows multiple users to connect and communicate with each other through the client.

## Usage

You can send messages to all connected peers and receive messages from them. Follow these guidelines:

- When you type a message, it will be displayed locally and sent to all connected peers.
- When a peer sends you a message, it will be displayed, and you will forward it to all other connected peers, excluding the sender.

Remember to include both the username and the message in the data sent over the network in the form "username: message". Each message should be null-terminated to indicate the end of the message.

## Implementation Details

### Input Callback Function

Modify the `input_callback` function to send a message to all connected peers when users press ENTER for a non-quit input. Ensure the message includes your username, a colon, a space, and the user's input. Terminate the message with a null character.

### Thread Functions

- Create a thread function to continuously check the listening socket for new connections.
- Create one or more separate thread functions to receive messages from peers, display the message locally, and forward it to other connected peers.

### Storage for Sockets

Use a data structure to store the sockets of connected peers. Ensure synchronization using pthreads mutex to prevent potential race conditions when multiple peers access the shared data structure.

### TCP/IP Networking Stack

Utilize the TCP/IP networking stack for all socket connections. Assume the hostname for all connections is localhost (IP address 127.0.0.1).

### Port Number

Allow the OS to choose the port number for the listening port. Use a value of 0 for the `sin_port` parameter and retrieve the chosen port using the `getsockname` system call. Pay attention to `htons` and `ntohs` function calls.

### Error Handling

Handle errors by checking return values of Linux system calls. In case of an error, read the integer value in `errno` to determine the issue. Refer to https://mariadb.com/kb/en/operating-system-error-codes/ for error code details.

## Debugging

To display text on the user interface for debugging, use the `sprintf()` and `ui_display()` functions. For example:

```c
char buffer[128];
sprintf(buffer, "INFO: Listening port number = %u", listening_port);
ui_display(buffer);
```

## Testing

Test the implementation with a simple network structure, where all peers are connected to a single central peer. Ensure that messages are forwarded through the network without crashes. Test the program with one central peer and at least two other connected peers.

## Functionality
- All peers display the username and message locally.
- Each peer can serve as a server for multiple clients and communicates with all connected peers.
- Each peer communicates with other peers or quits the chat network without crashing.
- Each peer keeps track of connected peers via a thread-safe storage mechanism.

