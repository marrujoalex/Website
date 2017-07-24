# Configuration files

Configuration files in Leopard can be any format. However, only JSON is included by default. Your Config file needs to be a `struct` conforming to the `Config` protocol.

From here, you can include other configuration related protocols, too. As your config file imports more configuration types, the more complex it gets and the more requirements there are to the file on the disk.

However, there's nothing preventing embedded, referenced (class) or even multiple config files being used.

## Leopard Configuration

Leopard comes with two default configuration file protocols.

`HTTPServerConfig` is used for configuring the HTTP server's port and hostname.

`RoutingConfig` allows customizing the token prefix for parameterized route compoenents.

You can inherit from both protocols, which would make you end up with the following:

```swift
struct ExampleConfig : Config, HTTPServerConfig RoutingConfig {
    var routeParameterToken: String? = ":"
    var port: UInt16 = 80
    var hostname: String = "0.0.0.0"
}
```
