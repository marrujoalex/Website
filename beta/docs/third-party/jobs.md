# Jobs by [Brett Toomey](https://github.com/BrettRToomey/Jobs)

Jobs is a minimalistic job system. It doesn't store the jobs cross-application, and doesn't allow restarting jobs after application exit.

It is, however, extremely useful for simple tasks such as sending a notification, ping or other non-critical jobs.

## SPM

Add the following dependency to your `Package.swift`

### Swift 4

```swift
.package(url: "https://github.com/BrettRToomey/Jobs.git", from: Version(1,0,0)),
```

### Swift 3

```swift
.Package(url: "https://github.com/BrettRToomey/Jobs.git", majorVersion: 1),
```

## Usage

Whenever you have a source file that needs to execute a Job, add the following import.

```swift
import Jobs
```

### Specifying time

All specified time is specified as a `Duration`.

Durations come in three types, `.seconds`, `.days` and `.weeks`.
Alternatively you can access them as properties on an int.

```swift
// The same
let oneMinute = 60.seconds
let alsoOneMinute = Duration.seconds(60)

// The same
let twoDays = 2.days
let alsoTwoDays = Duration.days(60)

// The same
let fiftyWeeks = 50.weeks
let alsoFiftyWeeks = Duration.weeks(50)
```

### Running a job

Running a repeating job can be useful, even if it's only used temporarily.

A job might send an update to a user over WebSocket every 30 seconds.

```swift
// Creates a job that executes every 30 seconds
let job = Jobs.add(interval: 30.seconds) {
  // execute job
}
```

### Stopping a job

Stopping a job is as simple as this:

```swift
job.stop()
```

### Autostart versus manual starting

Jobs start automatically by default, sometimes you need to start them when an event happens.

For example; a job might update clients over WebSocket only after the client sends a message requesting updates. But the job can be created in advance. It can also be stopped and restarted again.

```swift
// Creates a job that executes every 30 seconds
let job = Jobs.add(interval: 30.seconds, autoStart: false) {
  // execute job
}

job.start()
```
