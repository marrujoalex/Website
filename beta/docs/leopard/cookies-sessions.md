# Cookies & Sessions

Cookies play an important role in persistence. They allow the server and client to share a storage that can be modified by either party.

Cookies are often used for managing sessions.

## Reading a request's Cookies

```swift
let cookies = request.cookies

guard let sessionToken = String(cookies["session"]) else {
  ...
}
```

## Sessions

*TODO: Not implemented yet*
