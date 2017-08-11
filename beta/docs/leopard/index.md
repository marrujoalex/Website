# Leopard basics

Leopard is a high performance web framework, built 100% in Swift. It provides an easy to use async API.

This guide will help you set up Leopard from your terminal. It assumes Swift 4 has been set up.

## SPM - Package.swift

When creating a project using `swift package init --type=executable`, you'll notice that a set of files and directories have appeared in the current directory. One of these files is `Package.swift`, the project definition.

In here, a basic template will be shown. Let's add Leopard to that using:

```swift
.package(url: "https://github.com/OpenKitten/Leopard.git", .exact("1.0.0-beta1")),
```

And by adding `"Leopard"` to your target's dependencies.

```swift
// swift-tools-version:4.0
// The swift-tools-version declares the minimum version of Swift required to build this package.

import PackageDescription

let package = Package(
    name: "Example",
    dependencies: [
        .package(url: "https://github.com/OpenKitten/Leopard.git", .exact("1.0.0-beta1")),
    ],
    targets: [
        .target(
            name: "Example",
            dependencies: ["Leopard"]),
    ]
)
```

## A basic HTTP server

After `import Leopard` has been put on top of your `main.swift`, you'll need to create an `AsyncWebServer` like so:

```swift
// Create an HTTP server at port `80`
let server = try WebServer()

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
