# App

- Ensure a logging library is used and logs are written to a persistent
location (old logs can be removed as desired).
- Errors
    - Ensure errors are caught (at least as a near root-level try/catch).
        - Ideally, as few subjects will fall out when an exception occurs.
    - Ensure that errors are reported in an attention-getting workflow.
        - A log file on its own is not enough. For example, it could feed a
        monitoring/alerting solution, or there needs to be a ticket creation
        service class within the app itself.
    - Ensure subjects that fall out can be easily identified and reprocessed.
- Ensure network calls utilize resiliency.
- Ensure non-deterministic operations (file calls, network calls, retrieving
the current date/time) are either pushed to the boundary of the system or are
placed behind an abstract type.

