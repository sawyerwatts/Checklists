# Feature Hygine

When implementing a new feature within an application, here are some things that can be
considered:

- [ ] Primary logic
    - [ ] Prototype the happy path with a broad error handling strategy in mind
    - [ ] Implement error handling
    - [ ] Add assertions, especially around module boundaries
- [ ] Review the initial design, especially targetting the granularity's finer details, like that a
web API implements content negotionation, that the code is modularity, etc
    - [ ] See the relevant subpage in [./app/](./app/) based off granularity
- [ ] Implement resilience, cancellation, and the necessary amount of durability for the feature
- [ ] Review outbound network connections
    - [ ] Ensure connections are pooled appropriately (to ensure quicker requests and to avoid port
    exhaustion)
    - [ ] Ensure request timeouts are configured
    - [ ] Ensure connections periodically expire and need to be re-established (to protect against
    DNS changes)
- [ ] Ensure unmanaged resources are closed and check for other language-specific gotchas
- [ ] Ensure logging is sufficient
- [ ] Ensure performance and caching are sufficient
- [ ] Try to ensure the feature can live for a decade without becoming too legacy.
- [ ] Ensure inlined docs and wiki are written
- [ ] How do devs easily establish correctness of the code and regression test the code? (Automated
tests are usually the answer here).
    - [ ] If you haven't already, this would be a good time to abstract impurities.
    - [ ] Especially test the boundaries between modules, and between impurities.
    - [ ] Also rigorously test with various inputs, possibly with fuzzing.
- [ ] Monitoring: how do you ensure this feature keeps working?
- [ ] Ensure design decisions are documented.
- [ ] Consider if the README needs to be updated: is there anything that would be important for a
new dev to know?

