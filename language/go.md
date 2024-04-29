# Go

## Goroutines

- Is it possible for this concurrent operation to be started by the trunk
  Goroutine and handled off to a branch Goroutine, similar to an async/await
  function? This will help reduce the chances of leaking a Goroutine
- What is the successful shutdown condition/state of a sending Goroutine, if
  any?
  - Does the sending Goroutine need to close the channel? AKA does the receiving
    Goroutine need to be told that no more values will be sent?
- Does the receiving Goroutine early escape and put the sender at risk of
  hanging/leaking?
- Does the receiving Goroutine need to wait until the sending Goroutine has
  shutdown before escaping? (Done channels and waitgroups are helpful here.)
- Does every `select` block `case <-ctx.Done(): return` (or some other
  cancellation channel)?

