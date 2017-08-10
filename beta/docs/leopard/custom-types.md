# Custom Types

## ResponseRepresentable

ResponseRepresentable is useful when something needs to be used as a response.

Take your user model, for example. The implementation could take a template and generate the user profile page, after which it'd return the rendered webpage as a Response.

## BodyRepresentable

A BodyRepresentable is anything that is a content type of it's own. JSON is an excellent BodyRepresentable. BodyRepresentables are used in all bodies. In Lynx this is limited to the two implementations (Request and Response). But in SMTP you could also send a BodyRepresentable since it hinges on the Lynx API.

- JSON is not able to represent a Response since it won't hold the status code.
- JSON fits in a content type and usually takes up the whole body.
- JSON can be sent in both a Request and a Response

BodyRepresentables can implement themselves in two ways.

### Non-Streaming

Non-streaming BodyRepresentables write themselves in one go. String is a good exmaple of a non-streaming BodyRepresentable.

```swift
/// Makes String representative of a body
extension String : BodyRepresentable {
    /// Creates a body containing exclusively this String
    public func makeBody() throws -> Body {
        // Copy the binary data to a newly allocated buffer
        let allocated = UnsafeMutablePointer<UInt8>.allocate(capacity: self.utf8.count)
        allocated.initialize(from: [UInt8](self.utf8), count: self.utf8.count)

        let buffer = UnsafeMutableBufferPointer<UInt8>(start: allocated, count: self.utf8.count)

        // Creates a body that automatically deallocates the above buffer when necessary
        return Body(pointingTo: buffer, deallocating: true)
    }
}
```

### Streaming

Streaming BodyRepresentables implement a writer. A BodyRepresentable's writer can write the BodyRepresentable piece by piece.

A good example of a streaming BodyRepresentable is a file. When implementing your streaming BodyRepresentable, you'll call the writer with every buffer of data you want to send.

You also need to create a `makeBody` function that converts it to a bulk body, luckily we have a helper for that.

```swift
extension StreamingFile : BodyRepresentable {
    /// Creates a body from itself
    public func makeBody() throws -> Body {
        return Body(self.write)
    }

    /// Writes the entire body's data in one go
    public func write(_ writer: ((UnsafeBufferPointer<UInt8>) throws -> ())) throws {
        while let chunk = try self.nextChunk() {
          try writer(chunk)
        }
    }
}
```

## File

Files are simply BodyRepresentables with a file name and MIME type.

They can be sent over any connection, over multiple protocols and represent a more specific and descriptive piece of data.
