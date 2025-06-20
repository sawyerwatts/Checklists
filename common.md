# Common

## Misc

- Consider documentation: is there anything that would be important for a new
dev to know?
- Is this operation idempotent? If not, what is the point of no return? Can
it be made idempotent like via idempotency token HTTP headers?
- Under what situations could the code run out of memory?
- Are there any events that this code may want to push?
- As an exercise when making changes to an existing codebase, what would the
minimum required code changes be (if you didn't care about code quality,
modularity, ownership, etc)?
- See [./security.md][./security.md]

## Error Handling

- Ensure errors are caught (at least as a near root-level try/catch).
    - Ideally, as few subjects will fall out when an exception occurs.
    - Ensure subjects that fall out can be easily identified and
    reprocessed.
- Ensure that bound errors are reported, likely to a ticketing system.
    - A log file on its own is not enough. That said, it could feed a
    monitoring/alerting solution.
- Likely want to deliniate between developer- and business-bound errors.
- Be mindful of error sources, particularly other web APIs

## Scalability and SLAs

- How many requests per second?
- What is an estimated load over a period of time (the throughput)?
    - ops, reads, or writes per second
    - How many daily active users (DAU) (or targets per batch, etc) are
    expected?
- What is an acceptable latency or max run duration?
- Ideally, criteria for load and stress testing can be identified as well.
- What is an acceptable availability?
- What alerting is in place when an SLA is at risk or breached?
- What is the storage capacity?
- Is there a start-by time or a start-at time?
- Is there an end-by time or an end-at time?

