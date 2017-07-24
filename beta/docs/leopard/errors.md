# Errors

Errors are an important part of websites. They happen all the time. When authentication fails, the database connection drops or even when an unexpected condition triggers.

## Logging

Logging is an important part of your code, since it tells you what's happening at different levels. One might read verbose messages reporting every login attempt or just the warnings indicating a potential brute force.

Errors aren't easy to capture in a single "mold", everyone has their own method of organising errors.

By making your structs/classes `Encodable`, you enable the ability to log these entities to the currently selected logger/log destination.

```swift
struct LoginError : Error, Encodable {
  let username: String
}

Application.logger?.log(LoginError(username: "Joannis"), level: .warning)
```

## Changning the destination

Setting the logger can be done on the static property `logger` on `Application`.

```swift
Application.logger = MyCustomLogger()
```
