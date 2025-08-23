# Feature Hygine

When implementing a new feature within an application, here are some things that can be considered
(as with all of these checklists, the order isn't strictly relevant - it's a very fluid and cyclic
and iterative process):

- [ ] What is the context and goal for this feature?
- [ ] Define the broad error handling strategy
    - [ ] Do partial failures exist?
    - [ ] Do we care to destinguish errors between repeatable business errors, repeatable IT errors,
    and/or one-off/transient (IT) errors (AKA network issues)?
- [ ] Rough draft the happy path
    - When in doubt, pushing non deterministic ops outward usually ends well, to a point, especially
    to easily destinguish between repeatable vs non-repeatable errors.
    - Mutable, shared state between functions/classes/etc is generally the enemy
    - Consider parameters immutable (unless situationally very beneficial otherwise)
    - When dealing with gnarly state or data management problems, try to employ FP, but keep OOP and
    DI options in mind as well. FP-style may be be more brittle, but OOP will likely introduce
    shared state problems (and its use of obscured side effect outputs can make it less clear), so
    method DI / pass by reference can be a nice intermediate.
    - State machines can sometimes be a godsend
- [ ] Implement error handling
    - [ ] Bulkhead try/catch(es) as appropriate
    - [ ] Every line will evaluate to either an expression or throw an exception.
    - [ ] Under what situation will this line throw?
    - [ ] How will the feature behave if a given line throws?
- [ ] What are the different patterns / permutations of the input, output, and internal state? Are
they considered?
- [ ] Have all the output vectors been considered? Return, exception, out/mutated parameters, side
effects to services/infra, etc
- [ ] Ensure cancellation is supported
- [ ] Consider adding assertions and guards, especially around module boundaries
- [ ] Consider (re-)evaluating the [design](./design.md) of the written code
- [ ] See the relevant subpage in [./app/](./app/) as appropriate
- [ ] Review outbound network connections
    - [ ] Ensure connections are pooled appropriately (to ensure quicker requests and to avoid port
    exhaustion)
    - [ ] Ensure retries are implemented (jiggler, exponential backoff, and/or circuit-breaker)
    - [ ] Ensure request timeouts are configured
    - [ ] Ensure connections periodically expire and need to be re-established (to protect against
    DNS changes)
- [ ] Ensure unmanaged resources are closed and check for other language-specific gotchas
- [ ] Ensure logging is sufficient for observability (logging, metrics, tracing)
- [ ] How could this code run out of memory or disk?
- [ ] How could this block for a long time (like if paging isn't implemented)?
- [ ] How does the performance fare against [scalability and SLA
requirements](./scalabilityAndSla.md)?
    - Minimize network calls, then disk calls, then object allocations
- [ ] How does the implementation fare against [security concerns](./security.md)?
- [ ] How do devs easily establish correctness of the code and regression test the code? (Automated
tests and runnability are usually the answer here).
    - [ ] If you haven't already, this would be a good time to abstract impurities.
    - [ ] Especially test the boundaries between modules, and between impurities.
    - [ ] Also rigorously test with various inputs, possibly with fuzzing.
- [ ] Monitoring: how do you ensure this feature keeps working? See [operations.md](./operations.md)
for more.
- [ ] Ensure design decisions are documented.
- [ ] Ensure inlined docs, wiki, and README are up to date
    - [ ] Is there anything that would be important for a new dev to know?

