# App

- What type of app is this? What is that app type's checklist?

## Strategies

- What is the error handling strategy?
    - Ensure errors are caught (at least as a near root-level try/catch).
        - Ideally, as few subjects will fall out when an exception occurs.
        - Ensure subjects that fall out can be easily identified and
        reprocessed.
    - Ensure that bound errors are reported, likely to a ticketing system.
        - A log file on its own is not enough. That said, it could feed a
        monitoring/alerting solution.
    - Likely want to deliniate between developer- and business-bound errors.
    - Be mindful of error sources, particularly other web APIs
- How do devs easily 1) establish correctness and 2) ensure correctness is
preserved after making changes?
    - Automated testing is usually the answer here.
    - Integration tests are the best since they tend to cover more interactions
    and really test the extended behavior
- Ensure a logging library is used and logs are written to a persistent location
(old logs can be removed as desired).
    - Async logging is preferred
- Is the operation idempotent? Can the operation be reran? Is there a point of
no return?
- What is an estimated load and acceptable latency?
    - Ideally, criteria for load and stress testing can be identified as well.
- What are the SLAs?
    - What alerting is in place when an SLA is at risk or breached?
    - Is there a max duration?
    - Is there a start-by time or a start-at time?
    - Is there an end-by time or an end-at time?
- Termination
    - Is a crash-only mentality relevant or necessary here?
    - Are there any points of no return?
    - Does the app support ctl-c interrupts and cancellation?

## Modularity

- How well does each module isolate and abstract a design decision or
implementation detail?
- How's the locality of behavior?
- Is it clear why something should or should not be added to a module?
- Is the module's API small and clear?
- Dependency analysis: what does this module depend on and what modules depend
on this module?
- When in doubt, organize code by feature (and not by execution flow or
flowcharts).
- If a module could be sliced several ways (such as horizontal vs vertical
architectures), try to slice based off the design decision that will change more
frequently (so slice vertically as business logic will change much more
frequently than DB providers and data stores).

## Specifics

- Does the `README.md` detail:
    - The purpose, abstract, or problem statement of this application?
        - What are the critical characteristics?
        - What is this application *not*?
    - How to build, test, and deploy the code?
    - How are developer credentials stored, and when a new developer joins the
    project, how will they get the necessary credentials?
    - How is the bus factor mitigated?
        - Requirements, specs, diagrams, etc
        - Are there C4 diagrams (or any other diagrams)?
        - Where to find the extended/parent docs? Are they here or in a wiki?
    - What are the inputs and outputs/mutations/sideEffects of the app?
- Ensure network calls utilize resiliency.
- Ensure non-deterministic operations (file calls, network calls, retrieving the
current date/time) are either pushed to the boundary of the system or are placed
behind a swappable abstraction.
- Where the app will be deployed, how it will be deployed (ideally a pipeline),
and how will it be ran (and under what account, and what perms will that account
need (and does that account only have perms to what it needs))?
- Does the app make any events that we'd want to publish?

