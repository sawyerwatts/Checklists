# Operations

- [ ] What permissions will this app/operation require?
- [ ] What are the governance policies, if any?
- [ ] Is DevFinOps desired and applied?
- [ ] Are there any reports that we'd want written?
- [ ] What is the disaster recovery plan?

## Storage

- [ ] Are all persistent media (like logs) archived?
- [ ] When are archives moved to cold storage?
- [ ] Are there backups? 3-2-1?

## Recurring Tasks

- [ ] What is the cadence to perform dependency wellness checks?
- [ ] What is the cadence for updating packages?
- [ ] What is the cadence for updating the language itself?
- [ ] What is the cadence for updating the DB, OS, and other third-party services?
    - [ ] What is the patching policy?
- [ ] What is the cadence for performing a full restore in a fresh environment?
- [ ] What is the cadence for reviewing the documentation?
- [ ] Periodically review substantial decisions and preconditions
- [ ] Periodically review the design types, apps, and systems to ensure it makes sense at various
layers of abstraction

## Monitoring/Alering

- [ ] Are log levels warning/error/critical relayed to a ticketing system?
- [ ] How many/rate failures/errors is too many over what period?
- [ ] How many/rate successful requests/outputs/sideEffects?
- [ ] Is there monitoring of the various scalability and SLA requirements?
- [ ] VM/Container
    - [ ] Disk
    - [ ] CPU
    - [ ] RAM
- [ ] Cloud services: cost usage
- [ ] Are you tracking and dashboarding/alerting for response times, run times, etc?

