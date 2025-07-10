# Application Hygiene

When implementing an application from scratch, as a (mostly) separate exercise from system and app
design, here are some items to consider (the order is not super significant):

- [ ] Identify the app type(s) (console, daemon, CLI, web API, etc)
- [ ] Logging
    - [ ] Ensure logs are structured, and probably written as JSON
    - [ ] Ensure logs are written asyncronously
    - [ ] Ensure logs are written to a persistent location
    - [ ] Ensure older logs are moved to cold storage or otherwise cleaned
    - [ ] Ensure logs are monitored and alert as appropriate
- [ ] Ensure the app catches and relays termination and interrupt signals
- [ ] Ensure configurations (if only the environment-specific configs) can be loaded, like from
file, env var, flags, etc
- [ ] Ensure `README.md` is complete:
    - [ ] Document the purpose, abstract, or problem statement of this app
        - [ ] Document the critical characteristics
        - [ ] Document what the app does *not* do / be responsible for
    - [ ] Document how developer credentials are stored, and how a new developer on the project can
    get those credentials (if not using Docker Compose). If so, include the commands to start up and
    manage the services.
    - [ ] Document how to locally build and test
    - [ ] Document how the code should be deployed
    - [ ] Ensure the bus factor is mitigated
        - [ ] Requirements, specs, diagrams, etc
        - [ ] Ensure relevant C4 diagrams exist
    - [ ] Document the inputs and outputs/mutations/sideEffects of the app
    - [ ] Document how to perform common app updates, if any
    - [ ] Is there anything else that would be important for a new dev to know?
- [ ] Try to ensure the app can live for a decade without becoming too legacy.
- [ ] Operations: deployment and execution
    - [ ] Ensure the app is deployed automatically, ideally via a pipeline, else via a `makefile` or
    something
    - [ ] Ensure the app has an account/principle with the necessary permissions

