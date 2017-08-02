# Ocelot

Ocelot is a [Cheetah](../cheetah/index.md) based JWT implementation. It only covers JWS (JSON Web Signature), meaning all tokens are **signed, not encrypted**.

Any data in the JWT is readable but not writable by the client.

## SPM

Ocelot requires Swift 4 and the following dependency to be added:

```swift
.package(url: "https://github.com/OpenKitten/Cheetah.git", .revision("framework"))
```

## Creating a JSON Web Token

Creating a token is as simple as creating a `struct` and making it `Codable`.

```swift
struct AuthenticationToken : Codable {
  var username: String
}
```

Once you create an instance of this token, you can sign it using `JWSEncoder`. You'll need a (globally) defined secret in order to sign this JWT.

Please choose a random, high entropy, high amount of characters string for encoding the token.

```swift
import Ocelot

// Random, secure, private
// May not be known to anyone else
let secret = "DOn0tus3th1sPL3453"

// Create a new instance of the above codable token struct
let token = AuthenticationToken(username: "JoannisO")

// Signed using HMAC SHA512 and the above secret
let signedToken = try JWSEncoder.sign(token, signedBy: secret, using: .hs512())
```

The `signedToken` can then be given to the user, who can then carry this in a authorization bearer header, cookie or any other method you prefer for receiving the token.

## Deserializing the JSON Web Token to a struct

`JWSDecoder` can take the above `signedToken` and restore it back to a type-safe `struct`.

```swift
// Verifies the token, too, using the secet. If the token is invalid, an error will be thrown
let token = try JWSDecoder.decode(AuthenticationToken.self, from: signedToken, verifying: secret)

let works = token.username == "JoannisO" // true
```
