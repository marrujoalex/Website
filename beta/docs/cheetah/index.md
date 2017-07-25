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

Accessing nested types can be simply chained:

```swift
object["subObject"]["subSubObject"]["test"] = true
let test = Bool(object["subObject"]["subSubObject"]["test"])
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