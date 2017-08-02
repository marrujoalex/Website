# Middlewares

Leopard middlewares are exclusively asynchronous. This ensures quality between all middleware consumers.

## Middlewares

Alternatively you can create a normal, more complex Middleware which gives you direct control over the connection. This way you can implement protocol upgrades such as the included WebSocket implementation.

```swift
class MyComplexMiddleware : Middlware {
  func handle(_ request: Request, for remote: HTTPRemote, chainingTo handler: RequestHandler) {
    guard request.headers.bearer == "my-hardcoded-token" else {
      remote.send(Response(status: .internalServerError))
      return
    }

    handler(request, remote)
  }
}
```

This function does not allow you to throw errors. Instead, the HTTPRemote should be sent the Error so it can deal with it appropriately.
More about [HTTPRemote here.](../lynx/remote.md)
