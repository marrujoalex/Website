# Cheetah (JSON)

Cheetah is a fast library JSON. It's designed to read and write to strings and bytes. It's designed to have a familiar API for OpenKitten users.

## Creating a JSON Object or Array

```swift
let object: JSONObject = [
  "username": "Joannis",
  "age": 21,
  "admin": true,
  "number": 6.28,
  "powersOfTwo": [0, 2, 4, 6, 8, 10, 12, 14, 16],
  "subObject": [
    "test": true,
    "subSubObject": [
      "test": true
    ]
  ]
]

let array: JSONArray = [object, object, object]
```

## Working with objects

Objects are accessed using subscripts. Extracting a specific type is done by that type's initializer.

```swift
var object = JSONObject()

object["username"] = "Joannis"
object["age"] = 21
object["admin"] = true
object["number"] = 8.28

let username = String(object["username"]) ?? ""
let age: Int? = Int(object["age"])
let admin = Bool(object["admin"]) ?? false
let number: Double? = Double(object["number"])
```

## (de-)serializing JSON

You can easily create a JSONObject or JSONArray from a buffer.

```swift
let object = try JSONObject(from: buffer)
let array = try JSONArray(from: buffer)
```

The buffer can be `[UInt8]`, `Data` or `String`.

Serializing is equally easy:

```swift
let objectBytes: [UInt8] = object.serialize()
let arrayBytes: [UInt8] = array.serialize()

let objectString: String = object.serializedString()
let arrayString: String = array.serializedString()
```

## Nested objects

### Swift 3.1 or greater

Accessing nested types can be simply chained:

```swift
object["subObject"]["subSubObject"]["test"] = true
let test = Bool(object["subObject"]["subSubObject"]["test"])
```

### Swift 3.0

Swift 3.0 doesn't allow us to add the chaining descibed above. Instead, you'll have to unwrap and chain the subscripts manually.

```swift
JSONObject(JSONObject(object["subObject"])?["subSubObject"])?["test"] = true
let test = Bool(JSONObject(JSONObject(object["subObject"])?["subSubObject"])?["test"])
```

## Codable

Cheetah supports Codable, meaning you can conform any struct or class to `Encodable` for serializing, `Decodable` for deserializing and `Codable` for both.

This requires Swift 4.

### Encoding

```swift
import Cheetah

struct LoginResponse : Encodable {
  let token: String
  let success: Bool
}

let response = LoginResponse(token: "my-t0k3n", success: true)

let jsonObject = try Cheetah.JSONEncoder().encode(response)
print(jsonObject.serializedString()) // prints `{"token":"my-t0k3n","success":true}`
```

### Decoding

```swift
import Cheetah

struct LoginRequest : Decodable {
  let username: String
  let password: String
}

let request = try Cheetah.JSONDecoder().decode(LoginRequest.self, from: "{\"username\":\"test\",\"password\":\"hunter2\"}")

print(request.username) // prints "test"
print(request.password) // prints "hunter2"
```
