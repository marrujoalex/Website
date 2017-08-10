# Hashing

Hashing is the process of one-way encryption. It's an encryption that can't be reverted.

This is particularly useful for verifying information. One way hashes are employed is by hashing a message and verifying the message to output the same resulting hash on arrival. If the hash and/or message were tampered with or have been modified during transport, the results of hashing the file would not result in the same hash, making the information invalid.

The same is used for password hashing. The original password is not important, it poses a security risk on the user in case of data theft, for example, on a security breach of the information system (like a website).

Heavily and securely hashing a password is necessary to prevent brute forces.

## Support

CryptoKitten currently supports the following hashes:

- MD5
- SHA1
- SHA224
- SHA256
- SHA384
- SHA512

It also supports HMAC and PBKDF2 for message authentication and password hashing.

## Using a hash

All of the hashes described above have the same API and support two methods of hashing.

### 'Bulk' hashing

Hashing the entire message in one go is simple and natural, it doesn't require extra work. You'll need to enter the data as binary. This means Strings also need to be converted to binary. A common method of converting a string to binary is this:

```swift
let binaryMessage = [UInt8](binaryMessage.utf8)
```

To bulk hash binary data:

```swift
let hashedMessage0 = MD5.hash(binaryMessage)
let hashedMessage1 = SHA1.hash(binaryMessage)
let hashedMessage2 = SHA224.hash(binaryMessage)
let hashedMessage3 = SHA256.hash(binaryMessage)
let hashedMessage4 = SHA384.hash(binaryMessage)
let hashedMessage5 = SHA512.hash(binaryMessage)
```

Do *not* use basic hashes for passwords. They will be easily [brute-forced](https://en.wikipedia.org/wiki/Brute-force_attack).

### Incremental

Larger messages need to be copied around a lot. In a 1GB video, it means copying around 1GB of data. This makes the process unnecessarily really heavy. Instead of hashing the whole 1GB in one go, which would cost 2-3GB, you could pass around the buffer, for example, as a pointer.

When working with streams of incoming/outgoing data, you can also stream the data chunk by chunk to the hashing algorithm.

Before getting the hash, you'll need to finalize the hash, which will finish calculating the hash. This is *required*. Once you know the final block, you can pass it to finalize entirely.

```swift
// Create the context first
let context = MD5()

for fileData in file {
  context.update(fileData)
}

// The data to hash
let lastBlock: UnsafeBufferPointer<UInt8> = ...

// Hash the block of data
context.finalize(fileData)

// the hash as hexadecimal
let hash = context.hash.hexString
```

If the final block is already hashed, you still need to finalize it by calling finalize, even if there's an empty buffer of data to hash.

## Password hashing

For password hashing we currently only support PBKDF2 with HMAC.

### PBKDF2_HMAC

Hashing a password using PBKDF2 requires a hashing algorithm. You can select one generically like this:

```swift
// with MD5
PBKDF2_HMAC<MD5>

// with SHA1
PBKDF2_HMAC<SHA1>

// with SHA512
PBKDF2_HMAC<SHA512>
```

You can derive a key from the PBKDF2_HMAC. This requires three parameters.

The password is needed in binary. You can get the binary data from a string like below:

```swift
let binaryPassword = [UInt8](password.utf8)
```

Additionally, a *random* salt is needed. This salt *must* be randomly generated, *must* not be static and *must* be stored with the password in the database. Losing the salt means it's impossible to validate the stored (encrypted) password.

```swift
let hash = PBKDF2_HMAC<SHA512>.derive(fromPassword: binaryPassword, saltedWith: randomSalt).hexString
```

To validate it you can repeat the process.

```swift
let verifiedHash = PBKDF2_HMAC<SHA512>.derive(fromPassword: binaryPassword, saltedWith: randomSalt).hexString

let passwordCorrect = (hash == verifiedHash)
```
