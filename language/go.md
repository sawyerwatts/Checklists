# Go

- If unsure, check a factory's docs to see if there are clean up steps
necessary. Similarly, check if the result has a `Close()` method.
- If passing a struct to a func, it is assumed that it will not be updated
unless the docs say otherwise. Ensure this is the case.

## Goroutines and Channels

- When will this channel be happily closed? Are you at risk of trying to receive
  from or send to a closed channel?
  - Ex: "the lines channel will happily close after every line is scanned and
  sent/received through the channel"
- Since channels are a syncronization tool, can the receiver(s) early escape and
thus leave a hanging/leaked sender?
  - `context.WithCancel`, cancel funcs, and/or a buffered channel can help here
- Conversely, does the sending Goroutine early escape and need to communicate to
receiving Goroutine(s) that they may want to abort as well?
  - `wait/errgroup`s, `context.WithCancel`, cancel funcs, and/or a buffered
  channel can help here
- Similarly, do sibling-worker goroutines need to abort each other?
- Does the downstream/parent Goroutine need to wait until the upstream/child
Goroutines shut down before escaping?
  - Done channels and waitgroups can help here
- Does every channel interaction occur in a `select` block with a `case
<-ctx.Done(): return ctx.Err()` (or some other cancellation channel)?
- Channels will cascade completion/cancellation to downstream Goroutines via
closing; to cascade upstream, use `errgroup.WithContext` and register downstream
groups first

## HTTP Servers

You'll want to set time limits for max read/write/idle times, and max header
bytes. Otherwise, you can be DOSed.

```go
s := http.Server{
	Addr:           "localhost:8080",
	Handler:        router,
	ReadTimeout:    30 * time.Second,
	WriteTimeout:   90 * time.Second,
	IdleTimeout:    120 * time.Second,
	MaxHeaderBytes: 1 << 20,
}
```

## Interrupt Support

```go
quit := make(chan os.Signal, 1)
signal.Notify(quit, syscall.SIGTERM, syscall.SIGINT)
<-quit
ctx, cancel := context.WithTimeout(ctx, 5 * time.Second)
defer cancel()
// Now shut stuff down w/ new ctx
```

## Setting a defined timezone for the application

If it is desirable to explicitly control the timezone of all the `time.Time`s,
this can be done with the following code or via an environment variable.

```go
loc, err := time.LoadLocation("GMT")
if err != nil {
	panic(fmt.Sprintf("Couldn't set timezone to '%s'", "GMT"))
}
time.Local = loc
```

