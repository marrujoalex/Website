# Request & Response

Request and response are at the heart of HTTP, therefore they are extremely important. They've got two things in common: Headers and Body.

## Both Request and Response

Both Request and Response share similarities. One is the sender and the other the receiver. Requests get sent, responses get sent back.

### Headers

Headers contain metadata such as the timestamp, name and MIME-type of a file. Or such as the cookies carried over from previous sessions.

They can be instantiated with a literal or using an empty initializer.

```swift
let emptyHeaders = Headers()
let emptyHeaders2: Headers = [:]
```

Headers can be read and written.

```swift
let headers = Headers()

headers["Authorization"] = "bearer dasdwqjrnajdfnasdsiwq"
print(String(headers["Authorization"]))

let otherHeaders: Headers = [
  "Authorization": "bearer dasdwqjrnajdfnasdsiwq"
]
```

### Body

Bodies are a blob of undefined information. It may contain any kind of data, including but not limited to HTML, plain text, CSS, images, videos.

Writing to a body using a previously supported object is easy:

```swift
let testBody = try "test".makeBody()
```

Some of the supported body types are:

`Cheetah.JSONObject`, `Cheetah.JSONArray`, `String`, `Body`

Converting from a body to an existing type is simple to implement:

```swift
extension BodyRepresentable {
  func convertToCustomType() throws -> CustomType {
    let buffer = try self.makeBody().buffer

    return CustomType(from: buffer)
  }
}

let customEntity = try request.body.convertToCustomType()
```

JSONObject is already implemented for you, so you can try to read that from a Body using:

```swift
guard let object = request.jsonObject else {
  ... // handle data isn't a JSONObject
}
```

[More about JSON here.](../cheetah/index.md)

### BodyRepresentable

The other way around is useful, too. Converting your own type of data to a Body can be useful when you're writing your own templating tool, for example.

```swift
extension CustomType : BodyRepresentable {
  public func makeBody() throws -> Body {
    return Body(self.makeBytes()) // serialize this entity to binary
  }
}
```

## Only Request

Requests are unique in them carrying a method and a path. The path can contain query parameters in the URL. All of this allows for very specfic queries to be made.

### Method

Method is an enum. It exists of a specific type of action.

`GET` is used for fetching entities' their data.

`HEAD` is used for fetching entities' their metadata.

`POST` is used to create entities

`PUT` is used to replace created entities with a new entity

`PATCH` is used to update part of an entity

`DELETE` is used to delete an entity

Besides this, custom methods are also possible, or less commonly used methods.

### Path

The path describes the requested URL. It doesn't have many features publically available yet.

### Query parameters and Forms

Both query parameters and simple HTML forms use query encoding. This means that their API is equal.

```swift
guard let query: Query = request.body?.query else {
  // handle invalid body
}

guard let username: String = query["username"] else {
  // handle invalid request
}
```

### Multipart forms

*TODO: File uploading needs to be fixed before continueing documentation here*

## Only Response

Responses are unique in their response status codes. They're used to indicate the successfull execution of a request, or it's failure.

### Status code

HTTP status codes indicate one of five things.

* `1xx` an informational response
* `2xx` The operation was successfull
* `3xx` Redirecting is necessary
* `4xx` A client-side error occurred
* `5xx` A server-side error occurred

[More information on status codes is available here](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes)
