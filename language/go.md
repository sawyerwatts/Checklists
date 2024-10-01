# Go

- If unsure, check a factory's docs to see if there are clean up steps
necessary. Similarly, check if the result has a `Close()` method or similar.

## Goroutines and Channels

- When will this channel be happily closed?
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

