# Advanced routing

Routing can become more complex the more your application grows, so grouping routes and handlers is an essential part of any web framework.

We also support middlewares with the requirement of them being asynchronous.

## Grouping

Creating a group is similar to creating a route, only the route group get's returned.

```swift
let apiV1group = syncwebserver.grouped("api", "v1")

// GET `/api/v1/users`
apiV1Group.get("users") { request in
  ...
}
```

You can chain groups indefinitely.

```swift
let api = syncwebserver.grouped("api")

// `/api/v1/...`
let v1 = api.grouped("v1")

// `/api/v2/...`
let v2 = api.grouped("v2")

// `/api/v3/...`
let v3 = api.grouped("v3")
```

To improve the syntax, we also implement the `group` function with a trailing closure.

```swift
syncwebserver.group("api") { api in
  api.group("v1") { v1 in
    v1.get("users") { request in
      ...
    }
  }
}
```
