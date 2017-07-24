# Middlewares

Leopard middlewares are exclusively asynchronous, since Leopard's synchronous API is built on of the asynchronous one. This ensures quality between all middleware consumers.

## Creating a basic middleware

The following demonstrates the bearer token being compared against a hardcoded token. If this fails, an error is thrown.

If the bearer token is correct, the chain will be continued by calling `handle()`.

```swift
import Leopard

class MyMiddleware : BasicMiddleware {
  func handle(_ request: Request, chainingTo handle: (() -> (Future<ResponseRepresentable>))) throws -> Future<ResponseRepresentable> {
    guard request.headers.bearer == "my-hardcoded-token" else {
      throw InvalidBearerError()
    }

    return handle()
  }
}
```

## Complex middlewares

Alternatively you can create a normal, more complex Middleware which gives you direct control over the connection. This way you can implement protocol upgrades such as WebSocket. WebSocket, however, is included as part of Leopard.

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
