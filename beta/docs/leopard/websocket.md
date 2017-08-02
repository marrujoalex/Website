# WebSocket

WebSockets are useful when you want a real-time connection between client and server. WebSockets allow both parties to send each other information at any time. And support text as well as binary messages.

## Creating a websocket route

This example assumes you've got a variable `router` that is a `WebSocketRouter`.

```swift
router.websocket("api", "v1", "websocket") { client in
  // set up handlers
}
```

The handler will be called only when a successful WebSocket connection has been instantiated at this route. It doesn't do anything yet and will disconnect almost immediately.

## Adding a listener

```swift
router.websocket("api", "v1", "websocket") { client in
  client.onText { message in
    // echo the message back
    try client.send(message)
  }
}
```
