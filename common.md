# Common

## Misc

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

## Tackling Complexity through Simplicity and Modularity

*On the Criteria To Be Used in Decomposing Systems into Modules* by David Parnas is a great read.

*No Silver Bullet—Essence and Accident in Software Engineering* by Fred Brooks defines two forms of
complexity: essential and incidental.

*A Philosophy of Software Design* by John Ousterhout is a seminal in this section as it is the best
work I've encountered that discusses complexity and modularity in depth.

*Simple Made Easy* by Rich Hickey is great too.

Simplicity and modularity exist at all granularities, thus everything can be evaluated in terms of
simplicity and modularity/abstraction of its interface (comprised of its syntax and its
documentation). Cognative load is managed via information and information dependency management via
modules.

Cognative load is the amount of domain, language, codebase, etc knowledge necessary to understand
a piece of code.

- Simplicity: is when something is not tanged; it is a single track.
- Simplicity: how is the cognative load?
- Easiness: how obvious is the code, in a mechanical sense?

### Modularity

Conceptually, at many different granularities, like functions, classes, directories, and programs,
complexity and cognative load can be managed via encapsulation and dependency analysis (AKA
coupling). This is done by creating modules, de facto sources of truth for information
(requirements, invariants, implementation details, etc).

Temporal decomposition is when a system is decomposed based off the workflow being implemented.
These are not (necessarily) modules!

1. What is the information being encapsulated by this module?
    1. Is it obvious what this module does and does not (should and should not) contain?
1. Is the interface small (thus likely simpler for clients and a good encapsulation)?
    1. What are the important details?
    1. What are the unimportant details?
    1. Are these details' significance appropriately reflected in the interface?
1. Is the module deep (thus likely well encapsulated and/or providing value as an encapsulation)?
1. How is the cognative load? What are the dependencies, and how tight are they?
    1. Can anything be made more generic to help loosen the coupling and improve encapsulation?
    1. Is this the simplest interface with the least dependecies?
    1. How could this module be implemented without any given dependency?
1. What is the blast radius for, and/or difficulty in implementing, any particular change to the
encapsulated knowledge, implementation details, or dependencies?
    1. Consider what potential helpers could exist to better isolate different knowledge.
1. How is the cumulative complexity of the system's interfaces?

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

