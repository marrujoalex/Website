# Lynx

Lynx is a very basic HTTP server. It, optionally, has some basic routing functionality and by default routes all request to a single handler so it can be used in any way without enforcing limitations.

## Creating a webserver

Creating a basic webserver is simple. You provide a closure that handles all requests and clients.

The handler may not throw errors. Instead, errors should be passed to the client.

```swift
let server = try HTTPServer { request, client in
  // handle the request and respond to the client
}
```

When ready, you can start the server using `start()`

```swift
try server.start()
```

## Routing

```swift
let router = TrieRouter()
```

The router can be hooked up to a webserver simply.

```swift
let server = try HTTPServer(handler: router.handle)
```

Registering routes should only be done during set up. I *can* be used while serving, however, it *may* crash the server if a request is being handled during the addition of a route. A custom router can with a lock can be used to solve this problem.

### Registering a route

```swift
// Registers to `GET /path/to/route`
router.register(at: ["path", "to", "route"], method: .get) { request, client in
  // handle the request and respond to the client
}
```

## Responding

```swift
let response = Response(status: .ok)

do {
  try client.send(response)
} catch {
  client.error(error)
}
```

## Other information

[HTTP Request and responses](../leopard/request-response.md) is a useful place to learn more about the two main objects in HTTP.

Lynx does not support basic web framework features, it is designed to be simple.
