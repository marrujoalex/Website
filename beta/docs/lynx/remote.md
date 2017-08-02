# HTTPRemote

HTTPRemote is a protocol that handles HTTP Responses. It can be implemented using a custom implementation to levarage testability.

## Client

Client is the default remote TCP Client implementation. It handles serialization of responses and has an extension point for error handling.

### Error handling

`Client.errorHandler` is a variable that can be assigned a closure that handles errors that are `Encodable`. This is especially useful for responding to the Client with a JSON serialized Error.

For example:

```swift
// HTTP
import Lynx

// JSON
import Cheetah

Client.errorHandler = { error, client in
  dp {
    // Attempt to encode the error
    let jsonError = JSONEncoder().encode(error)
    let response = Response(status: 500, body: jsonError)

    // Send the error
    try client.send(response)

    // Close the connection
    client.close()
  } catch {
    // If the above failed, just close the connection
    client.close()
  }
}
```

## TestClient

TestClient is primarily useful in unit tests.

It accepts both an successful response and error handler

```swift
let client = TestClient({ response in
  XCTAssertEqual(response.status.code, 200)
}, or: { error in
  XCTFail("\(error)")
})
```

When you've created a testClient, you can pass it together with a request directly in the router or HTTP server.

```swift
let request = Request(method: .get, path: "/")

// Only need one of these 2
router.handle(request, for: client)
server.handle(request, for: client)
```
