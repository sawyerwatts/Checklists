# Go

A number of these come from the amazing *100 Go Mistakes and How To Avoid Them*
by Teiva Harsanyi.

- If unsure, check a factory's docs to see if there are clean up steps
necessary. Similarly, check if the result has a `Close()` method. You can also
check the type's declaration as well.
- If passing a struct to a func, it is assumed that it will not be updated
unless the docs say otherwise. Ensure this is the case.
- *All* file writes are buffered, and closing the file doesn't ensure the buffer
  is flushed. If this is unacceptable, use the `Sync` method (and then you can
  ignore the err from `Close`)
- Always read the `http` resp body, even if you don't use it. Otherwise, the
connex may be closed. Use `_, _ = io.Copy(io.Discard, resp.Body)` to most
efficiently read and discard the body
- When working with `http.Client`, recall that the default
`http.Transport.MaxIdleConnsPerHost` is 2. You may want to override this.
Also don't forget to configure timeouts.

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

## Testing and Benchmarks

- Ensure tests utilize `-parallel`/`t.Parallel` and `-shuffle`
- Ensure the tests are catagorized somehow, be it `-short`, build tags, or OS
env vars
- Be careful of expensive setup operations within a benchmark. The timer can be
reset and stopped/started as needed.
- To defend against inlining removing code, assign results to a local var, and
at the end of the benchmark, assign the local var to a global var

## Helpful Code Snippets

### HTTP Servers

You'll want to set time limits for max read/write/idle times, and max header
bytes. Otherwise, you can be DOSed.

```go
slogHandler := slog.NewJSONHandler(os.Stdout, &slog.HandlerOptions{AddSource: true})
slogger := slog.New(slogHandler)
s := http.Server{
	Addr:              "localhost:8080",
	Handler: http.TimeoutHandler(
		router,
		10_000*time.Millisecond,
		fmt.Sprintf("The request timed out as it ran longer than %n milliseconds", 10_000)),
	ReadHeaderTimeout: 500 * time.Millisecond,
	ReadTimeout:       500 * time.Millisecond,
	// WriteTimeout isn't configured since it closes the conn without
	// sending a response code and doesn't propogate the context.
	IdleTimeout:    30_000 * time.Millisecond,
	MaxHeaderBytes: 1 << 20,
	ErrorLog:       slog.NewLogLogger(slogHandler, slog.LevelError),
}
```

### Interrupt Support

```go
quit := make(chan os.Signal, 1)
signal.Notify(quit, syscall.SIGTERM, syscall.SIGINT)
<-quit
ctx, cancel := context.WithTimeout(ctx, 5 * time.Second)
defer cancel()
// Now shut stuff down w/ new ctx
```

### Setting a defined timezone for the application

If it is desirable to explicitly control the timezone of all the `time.Time`s,
this can be done with the following code or via an environment variable.

```go
loc, err := time.LoadLocation("GMT")
if err != nil {
	panic(fmt.Sprintf("Couldn't set timezone to '%s'", "GMT"))
}
time.Local = loc
```

