# Feature Hygine

When implementing a new feature within an application, here are some things that can be
considered:

- [ ] Primary logic
    - [ ] Prototype the happy path with a broad error handling strategy in mind
    - [ ] Implement error handling. Do partial failures exist?
    - [ ] Add assertions, especially around module boundaries
- [ ] Consider (re-)evaluating the [design](./design.md) of the written code, or at least these items:
    - [ ] How's the focus?
    - [ ] How's the clarity?
    - [ ] How's the cognative load?
    - [ ] How's the modularity? Is there semantic decoupling or only syntactic?
    - [ ] How's the conceptual cohesion?
    - [ ] How's the debuggability?
- [ ] See the relevant subpage in [./app/](./app/) as appropriate
- [ ] Implement resilience, cancellation, and the necessary amount of durability for the feature
- [ ] Review outbound network connections
    - [ ] Ensure connections are pooled appropriately (to ensure quicker requests and to avoid port
    exhaustion)
    - [ ] Ensure request timeouts are configured
    - [ ] Ensure connections periodically expire and need to be re-established (to protect against
    DNS changes)
- [ ] Ensure unmanaged resources are closed and check for other language-specific gotchas
- [ ] Ensure logging is sufficient
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

