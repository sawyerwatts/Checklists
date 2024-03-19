# App

- What is the purpose, abstract, or problem statement of this application?
- Have a strategy in place to allow devs to easily 1) establish correctness and
2) ensure correctness is preserved after making changes.
    - Automated testing is usually the answer here.
- What is the error handling strategy
    - Ensure errors are caught (at least as a near root-level try/catch).
        - Ideally, as few subjects will fall out when an exception occurs.
        - Ensure subjects that fall out can be easily identified and
        reprocessed.
    - Ensure that errors are reported, likely to a ticketing system.
        - A log file on its own is not enough. That said, it could feed a
        monitoring/alerting solution.
    - Deliniate between developer- and business-bound errors.
- Ensure a logging library is used and logs are written to a persistent
location (old logs can be removed as desired).
- Ensure network calls utilize resiliency.
- Ensure non-deterministic operations (file calls, network calls, retrieving
the current date/time) are either pushed to the boundary of the system or are
placed behind a swappable abstraction.
- Where the app will be deployed, how it will be deployed (ideally a pipeline),
and how will it be ran?

