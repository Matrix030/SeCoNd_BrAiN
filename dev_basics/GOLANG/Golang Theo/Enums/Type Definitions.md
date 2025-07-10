For all its faults, [TypeScript](https://www.typescriptlang.org/) _does_ have a pretty incredible type system. Here's one of the things it can do that I often miss in Go:

```typescript
type sendingChannel = "email" | "sms" | "phone";

function sendNotification(ch: sendingChannel, message: string) {
  // send the message
}
```

This `sendingChannel` type that we've created is a [union type](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#union-types). It can _only be one of the three strings_ that we've defined. That means when a developer calls `sendNotification()` they can't accidentally pass an invalid `sendingChannel` like "slack" or even a misspelled "emil". The TypeScript compiler will catch that mistake at compile time.

In Go, we don't have these nice things. We embrace the [Grug](https://grugbrain.dev/) mentality. The closest thing we have to a union type is a [type definition](https://go.dev/ref/spec#Type_definitions):

```go
type sendingChannel string

const (
    Email sendingChannel = "email"
    SMS   sendingChannel = "sms"
    Phone sendingChannel = "phone"
)

func sendNotification(ch sendingChannel, message string) {
    // send the message
}
```

It's a _bit_ safer than using a plain old `string` in Go, but it's not completely safe. Go will stop us from doing this:

```go
sendingCh := "slack"
sendNotification(sendingCh, "hello") // string is not sendingChannel
```

But it _will not_ stop us from doing this:

```go
// "slack" is automatically implied as a sendingChannel
sendNotification("slack", "hello")
```

And will also _not stop_ us from doing this:

```go
sendingCh := "slack"
convertedSendingCh := sendingChannel(sendingCh)
sendNotification(convertedSendingCh, "hello")
```

The `sendingChannel` type is just a wrapper for `string`, and because we made some constants of that type, most developers will just use those constants: we've made it easy to do the right thing. That said, Go still doesn't _force_ us to do the right thing like TypeScript does.