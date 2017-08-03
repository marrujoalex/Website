# TCP

Lynx also handles the implementation of the TCP sockets. This supports clients sockets with and without SSL.

This makes Lynx a solid basis for common web libraries, since many principles (such as MIME, headers, body and Multipart) are reused.

## Client

The plain and SSL TCP clients are almost identical in their API.

### Plain Client

Opening a simple socket using `TCPClient` is done by providing a hostname and port.

```swift
// Sets up the connection
let exampleClient = try TCPClient(hostname: "example.com", port: 127) { pointer, length in
  // process input
}

// Starts the connection, allowing read/write operations
exampleClient.connect()
```

The connection cleans up on deinitialization, meaning you don't need to close it manually.

### SSL Client

SSLClients are opened and used the same was as a `TCPClient`. SSLClient will use the Security framework on macOS, and OpenSSL on Linux.

```swift
// Sets up the connection
let exampleSSLClient = try TCPSSLClient(hostname: "example.com", port: 127) { pointer, length in
  // process input
}

// Starts the connection, allowing read/write operations
exampleSSLClient.connect()
```

### Receiving data

The trailing closure behind the `TCPClient` or `TCPSSLClient` is the on-read function. Since the sockets are built entirely in async, as our libraries do, input needs to be processed in the closure.

### Sending data

We currently accept 3 forms of sending data, which all boil down to the same.

```swift
let pointer: UnsafePointer<UInt8>
let length: Int

try client.send(data: pointer, withLengthOf: length)
```

```swift
let buffer: UnsafeBufferPointer<UInt8>

try client.send(buffer: buffer)
```

```swift
let bytes: [UInt8]

try client.send(bytes)
```
