# Welcome to OpenKitten

OpenKitten is a Server Side Swift ecosystem. It contains many libraries for creating backends and full stack swift applications.

We're active on [Slack](https://slackpass.io/openkitten) and [Github](https://github.com/OpenKitten/).

## The libraries

The libraries can be categorized into 3 categories.

### HTTP

[Leopard](/leopard/index.md) is a high performance web framework based on [Lynx](/lynx/index.md).

[Puma](/puma/index.md) is a [Lynx](/lynx/index.md) based HTTP client. Designed to work seamlessly with [Leopard](/leopard/index.md) .

[Lynx](/lynx/index.md) is the HTTP core, containing an HTTP server and all basic features of HTTP messages. It relies on [Schrodinger](/schrodinger/index.md) to provide clean asynchronous data flows.

### MongoDB

[MongoKitten](/mongokitten/index.md) is the fastest MongoDB driver. It's also the only pure-swift MongoDB driver. It relies heavily upon [BSON](/bson/index.md) and assumes familiarity with [BSON](/bson/index.md).

[Meow](/meow/index.md) is a codable ORM based on [MongoKitten](/mongokitten/index.md). It's flexible and boilerplate-free, allowing your models to stay clean.

### Utility

[Cheetah](/cheetah/index.md) is a JSON library that supports Swift 4 codables. In addition to that, it shares an API that fits right in with Swift and other OpenKitten libraries.

[Ocelot](/ocelot/index.md) is a JWT library that also supports codables for decoding JWTs. It leans on [Cheetah](/cheetah/index.md) for JSON data.

[Schrodinger](/schrodinger/index.md) is the promise library, used for asynchronous data flow.

[MailKitten](/mailkitten/index.md) is used for sending emails over SMTP.

## Third party

We're happy to work with the community and assist writing good libraries. Some problems are already solved by a third party without much room for improvement. These are such libraries.

The following libraries are not supported by us directly, although we advice using them if they're useful to you.

[Jobs](/third-party/jobs.md) is a minimalistic job system in Swift. It doesn't store jobs on application exit/restart.

## Requirements

These projects require the Swift 4 07-20 snapshot or the Xcode 9 build for macOS.

[Linux - Ubuntu 16.04](https://swift.org/builds/swift-4.0-branch/ubuntu1604/swift-4.0-DEVELOPMENT-SNAPSHOT-2017-07-20-a/swift-4.0-DEVELOPMENT-SNAPSHOT-2017-07-20-a-ubuntu16.04.tar.gz)

[Linux - Ubuntu 14.04](https://swift.org/builds/swift-4.0-branch/ubuntu1404/swift-4.0-DEVELOPMENT-SNAPSHOT-2017-07-20-a/swift-4.0-DEVELOPMENT-SNAPSHOT-2017-07-20-a-ubuntu14.04.tar.gz)

[macOS - Manual download](https://swift.org/builds/swift-4.0-branch/xcode/swift-4.0-DEVELOPMENT-SNAPSHOT-2017-07-20-a/swift-4.0-DEVELOPMENT-SNAPSHOT-2017-07-20-a-osx.pkg)

Or just install the latest Xcode 9 beta (beta 4 or greater).

### Linux

For SSL, Linux requires the installation of `libssl-dev`, `openssl` and Swift 4.
