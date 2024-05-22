# Go

## Goroutines

- What is the successful shutdown condition/state of a sending Goroutine, if
  any?
  - Does the sending Goroutine need to close the channel? AKA does the receiving
    Goroutine need to be told that no more values will be sent?
- Does the receiving Goroutine early escape and put the sender at risk of
  hanging/leaking? (`context.WithCancel` and cancel funcs are helpful here)
- Does the receiving Goroutine need to wait until the sending Goroutine has
  shutdown before escaping? (Done channels and waitgroups are helpful here)
- Does every `select` block `case <-ctx.Done(): return` (or some other
  cancellation channel)?

