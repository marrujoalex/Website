# MailKitten

MailKitten sends emails using SMTP.

## SPM

Add this dependency to your Package.swift

```swift
.package(url: "https://github.com/OpenKitten/MailKitten.git", .exact("1.0.0-beta1")),
```

## Connecting to SMTP

First, create a new SMTPClient object and specify whether to connect using SSL or not. It will take the default SMTP ports for SSL and non SSL.

```swift
let client = try SMTPClient(to: "smtp.example.com", ssl: true)
```

The SMTPClient object can only do one action at a time, do *not* use this to send multiple requests at the same time. Use multiple SMTPClients instead.

### Custom ports

Custom ports can be connected with, too.

```swift
let client = try SMTPClient(to: "smtp.example.com", at: 1234, ssl: true)
```

## Authenticating

After connecting, you have to authenticate manually. This way you can re-use the client to send emails from different accounts on the same server.

```swift
try client.authenticate("test@example.com", withPassword: "kittens")
```

## Creating an email

Creating an email requires a subject, type of contents, the contents themselves and the recipients.

The content type can be either `.plain` or `.html` for plaintext or HTML emails respectively.

The contents can be any type of body. Usually a String.

```swift
let scamEmail = Email(subject: "Bank transaction for 1 billion $$$",
                  type: .plain,
                  contents: """
                  I'm a nigerian prince in need of your bank account.
                  Send me a message back and I will transfer 1 billion $$$.
                  """,
                  recipients: "receiver-email@example.com")
```

To send this `scamEmail`, you simply call the `client.send` function.

```swift
let future = try client.send(email, from: "test@example.com")
```

The result of the `send` function is an empty future. It may contain an error, and will be completed when the email has been sent either successfully or unsuccessfully. You can `await()` this future to ensure the email has been sent correctly, or handle it otherwise [as described here.](../schrodinger/index.md)
