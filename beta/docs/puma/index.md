# Puma

Puma is an HTTP Client based on Lynx. Because it's build on Lynx it integrates with any extensions and features added on top o Lynx, such as JSON and file uploading.

## A single request/response

Puma is designed asynchronously, we only exposed the synchronous API so far.

A request is made the same way any of our requests are made [as described here.](../leopard/request-response.md)

You can then create an HTTP client:

```swift
// Creates a client for https://example.com
let response = SyncHTTPClient.send(request, toHost: "example.com", securely: true)
```
