# Schrodinger

Schrodinger is a library built for asynchronous programming. It works around a couple of simple concepts.

## Examples

### Heavy operations on a separate thread

The following Future spawns a separate thread that handles these heavy operations.

The resulting future is a `Future<Void>` since there's no value being returned from the closure.

Whenever you need to wait for the completion of the future, you can `.await()` it. Awaiting a future means you block the thread until it's complete.

Await will throw an error when the wait time has expired or if the executed closure had an error while executing. It will re-throw the captured error.

```swift
let future = Future {
  heavyOperation0()
  heavyOperation1()
  heavyOperation2()
}

lightOperation0()
lightOperation1()
lightOperation2()
lightOperation3()

// We need the heavy operations to be complete before continuing
// Throws an error if this doesn't complete in 30 seconds
try future.await(for: .seconds(30))
```

### Working with value containing futures

More often than not, you need to pass a result from the operation. This means that awaiting the value will return the returned value. Any errors thrown within the future

```swift
// This future will contain a string
let future = Future<String> {
  return try heavyOperationWithResult()
}

lightOperation0()
lightOperation1()
lightOperation2()
lightOperation3()

let heavyOperationResult = try future.await(for: .seconds(10))
```

### Handling futures asynchronously

Sometimes, awaiting the thread is unnecessary and an asynchronous handler can be used instead.

```swift
let future: Future<[User]> = try findUsersAsync()

future.onSuccess { users in
  // process the `[User]` (users)
  ...
}

future.onError { error in
  // handle error
  // for example `Application.logger?.log(error)` in Leopard
}
```

If you want more fine-grained control over both situations you can place a handler on both successful completions and errors.

```swift
let future: Future<[User]> = try findUsersAsync()

// result contains an enum
future.then { result in
  switch result {
  case .success(let users):
    // TODO: process users
  case .error(let error):
    // TODO: handle error
  }
}
```

Sometimes you want to assert the success of a result, that will let Schrodinger throw the error if an error occurred, or give you the successful result if the result was captured successfully.

```swift
let future: Future<[User]> = try findUsersAsync()

// result contains an enum
future.then { result in
  do {
    let users = try result.assertSuccess()

    // TODO: handle users
  } catch { error in
    // handle error
    // for example `Application.logger?.log(error)` in Leopard
  }
}
```

### Multiple handlers

Sometimes you'll want to watch for a result using multiple handlers. For example, a chat room with 5 users. One user sends a message and the other 4 await it's result.

That means the following is completely fine:

```swift
let future: Future<ChatMessage> = awaitForChatMessage()

// first handler
future.then { result in
  ...
}

// second handler
future.then { result in
  ...
}

// third handler
future.then { result in
  ...
}

// fourth handler
future.then { result in
  ...
}
```

### Future mapping and replacing

Whenever you're working asynchronously, awaiting a result alone can fall too short.

Sometimes you'll need to convert the result. Like a database result conversion to a model.

For this case, future mapping is useful.

```swift
let future: Future<DatabaseData> = try database.fetch(from: "users")

// maps the future asynchronously
let model: Future<Model> = try future.map { data in
  return try MyModel(from: data)
}
```

And in some scenarios, completing one future means starting another.

The following code results in a code pyramid:

```swift
let future: Future<DatabaseData> = try database.fetch(from: "users")

// maps the future asynchronously
let model: Future<Future<ProfileModel>> = try future.map { data in
  let model = try UserModel(from: data)

  let profile = Future<ProfileModel> = try model.getProfile()

  return profile
}

let profile = try model.await().await()
```

Which can be simplified using `replace`.

```swift
let future: Future<DatabaseData> = try database.fetch(from: "users")

// maps the future asynchronously
let model: Future<ProfileModel> = try future.replace { data in
  let model = try UserModel(from: data)

  let profile = Future<ProfileModel> = try model.getProfile()

  return profile
}

let profile = try model.await()
```
