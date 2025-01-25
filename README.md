# Checklists

In early 2024, Sawyer started reading *Code That Fits in your Head* by Mark
Seemann, which argues that developers should have checklists to help ensure they
do not forget important aspects of the software development lifecycle.
Critically, not every checklist item needs to be implemented because It Depends,
but the value is to ensure that all the items are considered and not forgotten.

This is an amalgamation of various sources Sawyer has seen online, in books,
and from experience.

Sawyer is a backend developer that primarily writes web APIs and console/batch
applications, and occassionally writes CLI tools. This is the perspective these
checklists are written from.

## Evergreen Drafts

Within a list, these steps don't need to be in complete isolation (like logging
can be done whenever earlier), but the goal is to ensure that an iteration is
always performed.

### App-Level

Here is a list of drafts to implementing the top level of an application. These
are much less strictly ordered than the operation-level drafts (even if that is
mildly not super important to perform in order).

1. Identify the app type(s) (console, daemon, CLI, web API, etc)
1. Logging
    - Ensure logs are structured, and probably written as JSON
    - Ensure logs are written asyncronously
    - Ensure logs are written to a persistent location
    - Ensure older logs are moved to cold storage or otherwise cleaned
1. Ensure the app handles termination and interrupt signals
1. Ensure configurations (if only the environment-specific configs) can be
loaded, like from file, env var, flags, etc
1. See relevant other pages
    - See [./common.md](./common.md) for more specific checks
    - See the relevant subpage in [./app/](./app/) based off granularity
1. Outbound network connections
    - Ensure connections are pooled appropriately (to ensure quicker requests
    and to avoid port exhaustion)
    - Ensure request timeouts are configured
    - Ensure connections periodically expire and need to be re-established (to
    protect against DNS changes)
1. Ensure connections (DB, HTTP, etc) are pooled with appropriate settings for
   the application.
1. Ensure `README.md` is complete:
    - Document the purpose, abstract, or problem statement of this app
        - Document the critical characteristics
        - Document what the app does *not* do / be responsible for
    - Document how developer credentials are stored, and how a new developer on
      the project can receive those credentials
    - Document how to locally build and test
    - Document how the code should be deployed
    - Ensure the bus factor is mitigated
        - Requirements, specs, diagrams, etc
        - Ensure relevant C4 diagrams exist
    - Document the inputs and outputs/mutations/sideEffects of the app
    - Document how to perform common app updates
1. Try to ensure the app can live for a decade without becoming too legacy.
1. Operations: deployment and execution
    - Ensure the app is deployed automatically, ideally via a pipeline, else via
    a `makefile` or something
    - Ensure the app has an account/principle with the necessary permissions
1. Operations: see [./operations.md](./operations.md)

### Operation-Level

Here is a list of drafts to implementing a new operation into a system.

1. Identify the granularity of the lower-level coding, like function, service,
module/package, web API, etc
1. Create the cursory design
1. Primary logic
    - Prototype the happy path
    - Implement error handling
    - Add assertions
1. How obvious is the code?
1. Implement resilience, cancellation, and the necessary amount of durability
for the use case
1. Ensure resources are closed and check for other language-specific gotchas
1. Ensure the written code is refactored into helpers
1. Ensure logging is sufficient
1. Ensure performance and caching are sufficient
1. Ensure code is readable
1. Review the initial design, especially targetting the granularity's finer
details, like that a web API implements content negotionation, that the code
is modularity, etc
    - See [./common.md](./common.md) for more specific checks
    - See the relevant subpage in [./app/](./app/) based off granularity
1. Try to ensure the operation can live for a decade without becoming too legacy.
1. Ensure inlined docs and wiki are written
1. How do devs easily establish correctness of the code and regression test the
code? (Automated tests are usually the answer here).
1. Monitoring: how do you ensure this feature keeps working?

