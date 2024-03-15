# App

- Ensure developers have the means to change the code with confidence (this
will probably require automated tests).
- Error handling strategy
    - Ensure errors are caught (at least as a near root-level try/catch).
        - Ideally, as few subjects will fall out when an exception occurs.
    - Ensure that errors are reported, likely to a ticketing system.
        - A log file on its own is not enough. That said, it could feed a
        monitoring/alerting solution.
        - Ensure developer- vs business-bound errors are deliniated.
    - Ensure subjects that fall out can be easily identified and reprocessed.
- Ensure a logging library is used and logs are written to a persistent
location (old logs can be removed as desired).
- Ensure network calls utilize resiliency.
- Ensure non-deterministic operations (file calls, network calls, retrieving
the current date/time) are either pushed to the boundary of the system or are
placed behind an abstract type.
- From the outset, identify where the app will be deployed, how it will be
deployed (probably a pipeline), and how it will be ran

