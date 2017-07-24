# Leopard basics

Leopard is a high performance web framework, built 100% in Swift. It provides an easy to use sync and async API.

This guide will help you set up Leopard from your terminal. It assumed Swift 4 has been set up.

## SPM - Package.swift

When creating a project using `swift package init --type=executable`, you'll notice that a set of files and directories have appeared in the current directory. One of these files is `Package.swift`, the project definition.

In here, a basic template will be shown. Let's add Leopard to that using:

```swift
.package(url: "https://github.com/OpenKitten/Leopard.git", .revision("master")),
```

And by adding `"Leopard"` to your target's dependencies.

```swift
// swift-tools-version:4.0
// The swift-tools-version declares the minimum version of Swift required to build this package.

import PackageDescription

let package = Package(
    name: "Example",
    dependencies: [
        .package(url: "https://github.com/OpenKitten/Leopard.git", .revision("master")),
    ],
    targets: [
        .target(
            name: "Example",
            dependencies: ["Leopard"]),
    ]
)
```

## A basic (synchronous) HTTP server

First you'll need to import the framework on top of your `main.swift`, like so:

```swift
import Leopard
```

This includes all necessary classes, structs and functions to get started.

In Leopard, there are three APIs to chose from. The Synchronous, Asynchronous and combined API.

```swift
// Create an HTTP server at port `80`
let server = try SyncWebServer()

// TODO: Set up routes

// start the server
try server.start()
```

The above will start a webserver without routes. So let's put some routes in the place of the `TODO: `

```swift
server.get("path", "to", "route") { request in
  return "Hello world"
}
```

The following creates a `GET /path/to/route` handler that always returns `"Hello world"`.

## A basic asynchronous HTTP server

After `import Leopard` has been put on top of your `main.swift`, you'll need to create an `AsyncWebServer` like so:

```swift
// Create an HTTP server at port `80`
let server = try AsyncWebServer()

// TODO: Set up routes

// start the server
try server.start()
```

This is almost identical to the sync webserver.

Basic routes are more complex than sync routes and provide no benefit with plaintext requests.

Async requests always require a future to be returned as a response.

```swift
server.get("path", "to", "route") { request in
  return Future { "Hello world" }
}
```

The following also creates a `GET /path/to/route` handler that always returns `"Hello world"`.

## Combining Sync and Async

For basic (plain) operations, synchronous is slightly faster than async, due to the overhead of the futures. However, when you mix external sources like databases and API calls into the formula, asynchronous requests not only become much lighter, but they reduce the impact on simultaneous requests by not blocking the thread.

For this reason, we support synchronous and asynchronous requests in harmony, side by side.

```swift
// Create an HTTP server at port `80`
let server = try WebServer()

// TODO: Set up routes

// start the server
try server.start()
```

To create a synchronous route in place of the `TODO:` you simply select the `sync` scope getter.

```swift
server.sync.get("path", "to", "route") { request in
  return "Hello world"
}
```

And when you need asynchronous behaviour for heavier operations:

```swift
server.async.get("path", "to", "route") { request in
  return Future { "Hello world" }
}
```
