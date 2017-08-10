# Multipart

Multipart is used for uploading complex forms, for example, those containing files.

## Creating a multipart form (HTML)

Forms require the addition of the "enctype" as displayed below.

```HTML
<form method="post" action="/path/to/handler" enctype="multipart/form-data">
  Favourite food: <input type="text" name="favouriteFood" /><br />
  File upload: <input type="file" />
  <button>Send</button>
</form>
```

## Parsing a request's multipart

We don't parse anything it until it's necessary, including Multipart.

```swift
guard let multipart = request.multipart else {
  ...
}
```

## Accessing named parts

In the above HTML form, the text input box had a 'name'.
You can use this name to easily access that part.

```swift
guard let favouriteFood = multipart["favouriteFood"] else {

}
```

## Accessing all parts

Unnamed parts can also be accessed. Multipart has a `parts` variable where you can find all parts.

```swift
for part in multipart.parts {

}
```

Every part has data and a type. The type can be either a value or a file.

Files carry their filename and MIME type with them.

## Handling the HTML form on the server

```swift
webserver.get("/path/to/handler") { request in
  return Future {
    // First, the multipart needs to be parsed
    guard let multipart = request.multipart else {
      return Response(status: 400)
    }

    // Food has a 'name', thus it's easily accessible
    guard let food = multipart["favouriteFood"]?.string else {
      return Response(status: 400)
    }

    // the file doesn't have a name, let's handle all files
    for part in multipart.parts {
      // if it's not a file, continue the loop
      guard case .file(let mime, let filename) = part.type else {
        continue
      }

      // The binary blob, or, contents of the file
      let body = try part.data.makeBody()

      // TODO: process file
    }
  }
}
```
