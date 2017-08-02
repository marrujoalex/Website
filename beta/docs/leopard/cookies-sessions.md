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

Session are currently supported through [JWT using Ocelot.](../ocelot/index.md)

JWT has proven to be flexible, easy and powerful at the same time. If you want simple token based sessions, we currently recommend managing the session in the [in-memory cache](cache.md) or the database of your choice.
