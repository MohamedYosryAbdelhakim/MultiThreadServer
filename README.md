# **Embedded Recruitment Task**

# Solution

# Rust Server with Multithreading and Client Handling

This project implements a multithreaded Rust server that can handle multiple clients simultaneously. The server processes messages from clients using Protocol Buffers (`prost`) and provides functionality for echoing messages and performing addition operations.

## Features and Improvements

### Key Features
- **Multithreaded Client Handling**: Each client connection is processed in its own thread, allowing the server to handle multiple clients concurrently.
- **Protocol Buffers Support**: Messages between the client and server are encoded and decoded using `prost`.
- **Graceful Client Disconnection**: The server handles client disconnections cleanly, ensuring resources are properly released.
- **Custom Messages**: The server supports two types of client messages:
  - `EchoMessage`: The server echoes back the received content.
  - `AddRequest`: The server performs addition on two integers and returns the result.

### Improvements in the New Code
1. **Thread Safety**: The `is_running` flag is wrapped in an `Arc<AtomicBool>` to allow safe access across threads.
2. **Error Handling**: Improved error messages and logging for better debugging.
3. **Graceful Shutdown**: The server can shut down cleanly by setting a flag to stop accepting new connections.
4. **Non-blocking I/O**: The server uses non-blocking mode to prevent stalling when no clients are connected.
5. **Message Decoding**: Enhanced decoding logic to handle various message types robustly.

## Implementation Details

### Server
The server listens for incoming TCP connections on a specified address and spawns a new thread for each client. It uses:
- `TcpListener` for accepting connections.
- `AtomicBool` to track the server's running state.

#### Main Responsibilities:
1. Accepting client connections.
2. Spawning threads to handle individual clients.
3. Managing server shutdown.

### Client
Each client connection is wrapped in a `Client` struct that manages the TCP stream. It:
- Reads incoming messages.
- Decodes messages using Protocol Buffers.
- Sends appropriate responses based on the message type.

#### Supported Messages:
- **EchoMessage**: Contains a string to be echoed back to the client.
- **AddRequest**: Contains two integers (`a` and `b`). The server responds with their sum.

### Test the Server
```bash
cargo test -- --test-threads=1
```
The server listens on `127.0.0.1:8080` by default.

### Sending Messages
Clients can send Protocol Buffer-encoded messages to the server. Use any TCP client or implement your own using the provided `.proto` definitions.

## Known Issues
1. **Message Encoding/Decoding**: Ensure the client encodes messages according to the `.proto` schema.
2. **Resource Management**: The server relies on the operating system to reclaim resources for terminated threads.
3. **Thread Lifetime**: Threads are not explicitly joined; they run until the client disconnects or an error occurs.

## Future Improvements
- **Thread Pooling**: Replace per-client threads with a thread pool to manage resources more efficiently.
- **Asynchronous I/O**: Use async Rust (`tokio` or `async-std`) for improved scalability.
- **Logging Enhancements**: Provide structured and configurable logging.
- **Comprehensive Tests**: Add unit and integration tests to verify functionality.
