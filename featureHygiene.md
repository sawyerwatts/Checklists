# Feature Hygine

When implementing a new feature within an application, here are some things that can be considered
(as with all of these checklists, the order isn't strictly relevant - it's a very fluid and cyclic
and iterative process):

- [ ] What is the context and goal for this feature?
- [ ] Define the broad error handling strategy
    - [ ] Do partial failures exist?
    - [ ] Do we care to destinguish errors between repeatable business errors, repeatable IT
    errors, and/or one-off (network) IT errors?
- [ ] Rough draft the happy path
    - Mutable, shared state between functions/classes/etc is generally the enemy
    - Consider parameters immutable (unless situationally very beneficial otherwise)
    - IO-less modules are likely going to be your friend, especially around error handling due to
    the innate distinction between repeatable and non-repeatable errors, but also that forces other
    design constraints onto the code.
    - When dealing with gnarly state or data management problems, try to employ FP, but keep OOP and
    DI options in mind as well. FP-style may be be more brittle, but OOP will likely introduce
    shared state problems (and its use of obscured side effect vectors can make it less obvious), so
    method DI / pass by reference can be a nice intermediate.
- [ ] Implement error handling
    - [ ] Bulkhead try/catch(es) as appropriate
    - [ ] Every line will evaluate to either an expression or throw an exception.
    - [ ] Under what situation will this line throw?
    - [ ] How will the feature behave if a given line throws?
- [ ] Add assertions and guards, especially around module boundaries
- [ ] Have all the output vectors been considered? Return, exception, out/mutated parameters, side
effects to services/infra, etc
- [ ] Implement resilience, cancellation, and the necessary amount of durability for the feature
- [ ] See the relevant subpage in [./app/](./app/) as appropriate
- [ ] Consider (re-)evaluating the [design](./design.md) of the written code, or at least these items:
    - [ ] How's the focus?
    - [ ] How's the clarity?
    - [ ] How's the cognative load?
    - [ ] How's the modularity? Is there semantic decoupling or only syntactic?
    - [ ] How's the conceptual cohesion?
    - [ ] How's the debuggability?
- [ ] Review outbound network connections
    - [ ] Ensure connections are pooled appropriately (to ensure quicker requests and to avoid port
    exhaustion)
    - [ ] Ensure request timeouts are configured
    - [ ] Ensure connections periodically expire and need to be re-established (to protect against
    DNS changes)
- [ ] Ensure unmanaged resources are closed and check for other language-specific gotchas
- [ ] Ensure logging is sufficient
- [ ] How could this code run out of memory or disk?
- [ ] How could this block for a long time (AKA paging isn't implemented)?
- [ ] How does the performance fare against [scalability and SLA requirements](./scalabilityAndSla.md)?
- [ ] How does the implementation fare against [security concerns](./security.md)?
- [ ] Try to ensure the feature can live for a decade without becoming too legacy.
- [ ] How do devs easily establish correctness of the code and regression test the code? (Automated
tests are usually the answer here).
    - [ ] If you haven't already, this would be a good time to abstract impurities.
    - [ ] Especially test the boundaries between modules, and between impurities.
    - [ ] Also rigorously test with various inputs, possibly with fuzzing.
- [ ] Monitoring: how do you ensure this feature keeps working?
- [ ] Ensure design decisions are documented.
- [ ] Ensure inlined docs, wiki, and README are up to date
    - [ ] Is there anything that would be important for a new dev to know?

